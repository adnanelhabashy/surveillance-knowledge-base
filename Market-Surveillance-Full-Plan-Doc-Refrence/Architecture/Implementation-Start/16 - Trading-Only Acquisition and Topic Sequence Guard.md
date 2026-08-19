---
id: IMPL-START-16
type: architecture
status: active-runtime-profile
tags:
  - surveillance/implementation
  - surveillance/ingestion
  - surveillance/coverage
  - drop/kafka
---

# Trading-Only Acquisition and Topic Sequence Guard

## Decision

THE EYE ingestion consumes **trading and live market-context topics only**.

Pure reference/identity topics such as investors, accounts, participants, actors, assets and order books are not consumed by this ingestion worker. The current platform already projects that reference state into Redis through `ReferenceDataCacheService`.

This reduces unnecessary Kafka traffic and parsing work in the real-time surveillance path while keeping the market events needed for order-book reconstruction and manipulation detection.

See [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]].

## Configured trading/context topics

The active list lives in:

```text
TheEye.Ingestion/appsettings.json
TopicConsumption:Topics
```

Current starting profile:

```text
mme.drop.parsed.startoftransaction
mme.drop.parsed.commit
mme.drop.parsed.orders
mme.drop.parsed.rejectedorders
mme.drop.parsed.trades
mme.drop.parsed.offexchangetrades
mme.drop.parsed.tradestatistics
mme.drop.parsed.circuitbreakerinfo
mme.drop.parsed.quoterequests
mme.drop.parsed.quoterequestresponses
mme.drop.parsed.indicativequotes
mme.drop.parsed.indicativequoteoffers
mme.drop.parsed.bestbidoffers
mme.drop.parsed.equilibriumprices
mme.drop.parsed.indexprices
mme.drop.parsed.pricelimits
mme.drop.parsed.referenceprices
mme.drop.parsed.awaymarketbbo
mme.drop.parsed.delayedlastmatchprices
mme.drop.parsed.sessionchanges
mme.drop.parsed.marketannouncements
mme.drop.parsed.businessdatechanges
mme.drop.parsed.repoorderbookstatuses
```

`referenceprices` remains because it is **live market-price context**, not identity/reference-master data.

## Deliberately excluded from this ingestion worker

```text
mme.drop.reference.*
mme.drop.parsed.endofreferencedata
mme.drop.parsed.initialbusinessdates
mme.drop.parsed.corporateactions
mme.drop.parsed.exchangerates
mme.drop.parsed.accountpositionupdates
```

The identity/reference values required by downstream surveillance are read from the existing Redis reference cache. If a detector later needs one of the excluded dynamic domains as a first-class event stream, add it explicitly to configuration rather than returning to an unconditional all-topic subscription.

## Why the old global-gap algorithm must change

`MmeSequenceNumber` is global across DROP message types. Once THE EYE intentionally excludes reference topics, its observed global sequence becomes sparse by design.

Example:

```text
1405 Order       <- selected
1406 Investor    <- excluded, available in Redis
1407 Account     <- excluded, available in Redis
1408 Participant <- excluded, available in Redis
1409 Trade       <- selected
```

For the selected trading profile:

```text
1405 -> 1409
```

is **not** a surveillance feed gap.

Therefore:

- `MmeSequenceNumber` remains the global source ordering/evidence value;
- selected events are still released in relative MME order;
- omitted MME values are skipped in sparse assembly mode;
- selected-topic continuity is proven using the new per-topic sequence headers.

## Per-topic sequence contract

### `topic-sequence-number`

```text
8-byte binary UInt64
little-endian
```

Current producer encoding is equivalent to:

```csharp
BitConverter.GetBytes(ulongValue)
```

THE EYE production default accepts only this binary format. ASCII decimal parsing exists as an explicitly configurable compatibility fallback and is disabled by default.

### `topic-sequence-epoch`

```text
ASCII / UTF-8 text
32 lowercase hexadecimal characters
normally Guid.ToString("N")
```

Example:

```text
0123456789abcdef0123456789abcdef
```

### Identity

The topic sequence is interpreted as:

```text
KafkaTopic
+ TopicSequenceEpoch
+ TopicSequenceNumber
```

Never use `topic-sequence-number` without its epoch.

See [[15 - MME Sequence Header Encoding Verification|Kafka Sequence Header Encoding Verification]].

## Immediate gap check

The hot-path guard stores the last observed sequence for every `(topic, epoch)` pair.

```text
first value for pair      -> establish baseline
last + 1                  -> continuous
new > last + 1            -> TopicSequenceGapEvent immediately
new <= last               -> replay/duplicate; do not move frontier backward
new epoch                 -> independent sequence frontier
```

Example:

```text
orders epoch A: 500, 501, 504

missing topic sequence = 502..503
```

The gap can be raised as soon as `504` arrives. It does not wait for the slower global MME watermark.

## One-partition rule

The verified current Kafka baseline is one partition per DROP topic.

The current `TopicSequenceGuard` therefore requires one partition per configured topic when enabled. Startup fails if a configured topic has multiple partitions and this guard is still configured as single-partition.

If Kafka is later repartitioned, define whether topic sequence is:

```text
one sequence across the whole topic
or
one sequence per Kafka partition
```

before changing the guard. Do not guess.

## Global selected-event ordering

The source assembler still uses `MmeSequenceNumber` to preserve relative source order among selected events.

Because excluded topic messages create intentional MME holes, the assembler runs with:

```text
SourceAssembly:RequireContiguousMmeSequence = false
```

The existing ingestor Redis watermark remains a **release-ordering guard**, not the primary selected-topic gap detector.

The watermark reads are pipelined/parallelized so the low refresh interval does not create six serial Redis waits.

## Performance configuration

The starting low-latency profile is configuration-driven:

```text
Kafka.Consumer.FetchWaitMaxMs = 10
Kafka.Consumer.FetchMinBytes = 1
Kafka.Consumer.QueuedMinMessages = 100000
Kafka.Consumer.QueuedMaxMessagesKbytes = 131072
SourceAssembly.ConsumeTimeoutMs = 25
SourceAssembly.WatermarkRefreshIntervalMs = 25
Kafka.Producer.LingerMs = 1
Kafka.Producer.CompressionType = Lz4
```

Intent:

- low Kafka fetch wait for live latency;
- large local prefetch queue for catch-up/replay throughput;
- no auto-commit;
- fast in-memory topic gap check;
- small producer linger while retaining batching/compression;
- canonical and audit publishes run concurrently.

These are starting values and must be tuned from measured broker/CPU/lag telemetry, not treated as permanent magic numbers.

## `appsettings.json` ownership

Runtime behavior is externalized instead of hard-coded:

```text
Kafka broker
consumer group
consumer fetch/queue/timeouts
producer acks/batching/compression
output topic names
selected source topics
Redis connection
source assembly timing
strict/sparse MME mode
topic-sequence header names and compatibility mode
ingestor watermark instances and Redis key formats
```

This allows environment overrides without recompiling the service.

## Oracle sequence-guard summary review

Current table shape:

```sql
DROP_MESSAGE_SEQUENCE_GUARD_SUMMARY
```

The column types fit the current contract:

```text
MME_SEQUENCE_NUMBER   NUMBER(20)
TOPIC_SEQUENCE_NUMBER NUMBER(20)
TOPIC_SEQUENCE_EPOCH  VARCHAR2(32 CHAR)
```

`NUMBER(20)` can represent the decimal range required by `UInt64`, and the epoch column exactly fits the 32-character contract.

The current primary key:

```text
(TOPIC_NAME, MME_SEQUENCE_NUMBER)
```

is useful for source-record uniqueness, but it does **not** enforce the new per-topic sequence identity.

Recommended additional uniqueness for the current one-topic-sequence model:

```sql
ALTER TABLE DROP_MESSAGE_SEQUENCE_GUARD_SUMMARY
  ADD CONSTRAINT UQ_DROP_MSG_TOPIC_SEQ
  UNIQUE (TOPIC_NAME, TOPIC_SEQUENCE_EPOCH, TOPIC_SEQUENCE_NUMBER);
```

If topic sequence later becomes partition-local, revisit the contract and likely include `KAFKA_PARTITION` in that uniqueness key.

Do not query Oracle for every live gap decision. Oracle is audit/reconciliation storage; the hot path compares Kafka headers in memory.

## Failure behavior

Malformed or missing required topic-sequence metadata:

```text
-> data-quality event
-> coverage marked degraded
-> do not invent a replacement value
```

Observed forward topic gap:

```text
-> TopicSequenceGapEvent on surv.coverage.v1
-> keep processing later market events
-> coverage stays honest/degraded
```

The first record seen for a `(topic, epoch)` establishes the in-process baseline. Persisting topic frontiers across process restarts is a separate hardening item if the platform requires gap proof across a restart boundary without replaying an earlier Kafka record.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[08 - DROP Event Acquisition Matrix|DROP Event Acquisition Matrix]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[15 - MME Sequence Header Encoding Verification|Kafka Sequence Header Encoding Verification]]
- [[DROP-Current-System/08 - Kafka Topic Catalog|Current Kafka Topic Catalog]]
- [[DROP-Current-System/09 - Redis State and Reference Cache|Current Redis State and Reference Cache]]
