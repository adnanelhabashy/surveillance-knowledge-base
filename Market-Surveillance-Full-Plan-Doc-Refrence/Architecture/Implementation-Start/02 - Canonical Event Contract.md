---
id: IMPL-START-02
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/contracts
---

# Canonical Event Contract

> [!IMPORTANT]
> `surv.drop.canonical.v1` is the **source-truth boundary** between `TheEye.Ingestion` and `TheEye.Silo`. Canonicalization preserves DROP meaning and source evidence; it does not perform cross-event reference enrichment, trade pairing, transaction projection or market-state reconstruction.

See [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]].

## Goal

Give THE EYE one stable contract around current DROP without flattening away source semantics or forensic evidence.

```text
Canonical source envelope
+ exact typed DROP payload
```

## Current canonical envelope shape

Conceptually:

```csharp
public sealed record DropEventEnvelope<TPayload>
{
    public required string EventId { get; init; }
    public required string EventType { get; init; }
    public required int SchemaVersion { get; init; }

    public required string Source { get; init; }
    public required string SequenceDomain { get; init; }
    public required string SequenceEpoch { get; init; }
    public required ulong MmeSequenceNumber { get; init; }

    public required short MessageGroup { get; init; }
    public required short MessageId { get; init; }
    public required byte DropPartitionId { get; init; }

    public required DateTimeOffset EventTime { get; init; }
    public required DateTimeOffset ReceiveTime { get; init; }

    // Nullable routing/index hints extracted from this source payload only.
    public int? OrderBookId { get; init; }
    public int? AssetId { get; init; }
    public int? ParticipantId { get; init; }
    public int? ActorId { get; init; }
    public int? AccountId { get; init; }
    public int? InvestorId { get; init; }
    public long? OrderId { get; init; }
    public long? MatchId { get; init; }

    public required string KafkaTopic { get; init; }
    public required int KafkaPartition { get; init; }
    public required long KafkaOffset { get; init; }

    public required TPayload Payload { get; init; }
}
```

The exact physical DTO may contain additional operational fields, but the wire rule is unchanged: **source identity + natural routing hints + source/receive time + Kafka evidence + typed source payload**.

`PayloadObject`/runtime helper properties are not part of the JSON wire contract.

## No cross-event enrichment in the Ingestor

The Ingestor may extract a field only when it is naturally available from that source message.

Examples:

```text
Investor message -> InvestorId may be present
Account message  -> InvestorId may be present
Order message    -> ActorId / ParticipantId / OrderBookId / OrderId are natural
Trade message    -> ActorId / ParticipantId / OrderBookId / OrderId / MatchId are natural
```

But the Ingestor must **not** do this:

```text
Order.account
  -> query Account reference
  -> derive InvestorId
  -> mutate/enrich canonical Order
```

That Account → Investor resolution belongs to the Silo reference projector.

## Transaction and business-date context

These are **Silo-side projections**, not required Ingestor source enrichment.

Canonical source events preserve the native transaction/date messages:

```text
StartOfTransaction
Commit
InitialBusinessDate
BusinessDateChanged
```

The Silo can deterministically derive:

```text
TransactionContext
BusinessDateContext
```

while replaying the same ordered canonical stream.

If a future derived envelope adds transaction/business-date context, it must retain lineage to the original canonical source events and must not redefine the source payload.

## Sequence fields

Keep separate:

```text
MmeSequenceNumber
    MME source ordering evidence used by THE EYE

DropPartitionId
    DROP protocol partition identity

MarketAnnouncement.sequenceNumber
    announcement-specific payload field

KafkaOffset
    transport position inside one Kafka topic partition
```

Never substitute one for another.

## Kafka header encoding correction

`MmeSequenceNumber`, message group/id and DROP partition are transported by the existing application using Kafka headers.

The Nasdaq DROP specification defines the **payload's** little-endian binary representation. It does **not** define the Kafka-header serialization used by the existing MME ingestors.

The first live Ingestor record disproved the original fixed-width assumption for all headers.

Therefore:

- the real Kafka header encoding must be confirmed from live evidence/source code;
- decoding remains isolated in `DropSourceRecordContextFactory`;
- unknown encodings are quarantined with raw evidence;
- no source value is invented when decoding fails.

## SequenceDomain and SequenceEpoch

`SequenceDomain` names the actual source namespace that owns the MME sequence.

Do not derive it from topic, message family, order book, actor or DROP partition without proof.

`SequenceEpoch` prevents collisions when sequence values reset. Its reset semantics remain a Phase-0/P0 open item; do not treat business date as the epoch until verified.

## Deterministic EventId

Current design:

```text
SHA-256(
  Source,
  SequenceDomain,
  SequenceEpoch,
  MmeSequenceNumber,
  MessageGroup,
  MessageId,
  DropPartitionId
)
```

No random GUID for replayable source events.

## Header/payload validation

For typed payloads:

```text
drop-group-id     == payload.messageGroup
drop-message-id   == payload.messageId
drop-partition-id == payload.partitionId
```

Mismatch => `SourceMetadataMismatchEvent` in data quality and the source record is not admitted to the strict canonical stream.

## Event time

Map the semantically correct source timestamp to `EventTime` while retaining the native source field inside `Payload`.

Examples:

```text
Order             -> timeChanged
Trade             -> tradeTime
RejectedOrder     -> rejectTime
BestBidOffer      -> timestamp
SessionChange     -> timestamp
AccountPosition   -> timestamp
```

`ReceiveTime` is ingestion observation time, not a replacement for exchange/source time.

## Important source payload semantics

### `OrderLifecycleEvent`

Preserve the official rich Order payload: lifecycle status/before-status, changeReason, quantities, order ownership, order type/capacity, account/custodian/customer information, orderToken and trigger/peg fields.

A derived New/Modify/Cancel interpretation is downstream convenience, not a replacement for native fields.

### `TradeSideEvent`

Official `Trade [20]` is one side of a trade. Preserve `matchId`, side, actor/participant, order, price/quantity, dealSource, passiveAggressive, account/custodian and lifecycle/report fields.

`MatchedTradeEvent` is produced later by the Silo `TradePairProjector`.

### Reference events

Investor, Account, Actor, Participant, Asset, OrderBook and other reference messages remain independent canonical source events.

Investor resolution across accounts is Silo-side derived state.

## Derived events

Derived events keep source lineage:

```text
MatchedTradeEvent
ResolvedOrderEvent
ResolvedTradeEvent
OrderBookStateChangedEvent
AuctionStateEvent
PositionStateChangedEvent
FactBundle
SurveillanceAlertEvent
```

Conceptual evidence:

```text
SourceEventIds[]
SourceSequenceMin/Max
CoverageEpochId
CoverageDegraded
```

These are downstream of `surv.drop.canonical.v1`.

## Kafka evidence

For each source event retain:

```text
KafkaTopic
KafkaPartition
KafkaOffset
```

MME identity answers **what source event this is**. Kafka coordinates answer **where this delivered evidence was consumed**.

## Canonical topic ordering

For the initial production version:

```text
one surv.drop.canonical.v1 partition per SequenceDomain
```

The Ingestor publishes released events in increasing MME sequence order; the Silo consumes that ordered lane sequentially before keyed parallel dispatch.

## Replay/versioning

- `SchemaVersion` mandatory.
- Source `EventId` stable across replay.
- Additive schema evolution preferred.
- Silo state application idempotent by `EventId`.
- Replay may duplicate transport; duplicate state effects are not allowed.

## Data-quality rule

If required source identity/order metadata cannot be proven:

```text
preserve forensic evidence
emit durable data-quality condition
mark coverage/evaluability appropriately
```

Do not manufacture values.

## Navigation

- [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[08 - DROP Event Acquisition Matrix|DROP Event Acquisition Matrix]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
