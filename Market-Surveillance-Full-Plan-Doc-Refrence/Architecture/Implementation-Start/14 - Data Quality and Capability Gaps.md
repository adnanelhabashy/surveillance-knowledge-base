---
id: IMPL-START-14
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/data-quality
  - surveillance/coverage
---

# Data Quality and Capability Gaps

> [!IMPORTANT]
> Raw source quality and source-gap decisions are owned by `TheEye.Ingestion`. Reference/domain evaluability is owned downstream by the Silo/rules layer. See [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]].

## Purpose

A surveillance system must distinguish:

```text
No suspicious behavior observed
from
Required data was missing / incomplete / not connected
```

## Coverage dimensions

Track independently:

```text
FeedCoverage       -> did expected source messages arrive?
ParseCoverage      -> could source records be decoded/identified?
ReferenceCoverage  -> was required context resolvable as-of source sequence?
DomainCoverage     -> were required external domains connected/healthy?
```

## Current live findings

The current Ingestor live test has already exposed these real gaps:

- only **22/37 documented source topics** were present on the tested broker;
- the original fixed-width Kafka-header decode assumption was wrong on the first live record;
- `SequenceEpoch` reset semantics remain unverified;
- `mme.drop.parsed.unhandled` and raw-message DLQ are not wired into the Ingestor yet;
- distinct same-sequence conflicts are currently detected/logged but still need a first-class durable event contract;
- current source-offset commit timing can acknowledge a record while its only accepted copy is still in the in-memory reorder buffer. This is a P0 reliability risk.

## Source integrity events

### MalformedSourceHeader / MissingSequenceHeader

A source Kafka record cannot produce valid required source metadata.

Current behavior: malformed headers are logged with raw evidence, quarantined to `surv.dataquality.v1`, coverage is degraded and processing continues.

Rules:

- preserve Kafka topic/partition/offset;
- preserve header names/lengths/hex/text evidence;
- do not synthesize source sequence or protocol identity;
- unknown header encoding remains a source-quality issue until the real producer contract is confirmed.

### SourceMetadataMismatchEvent

Decoded Kafka DROP identity disagrees with typed payload group/id/partition.

Action:

- retain both values and payload evidence;
- publish data-quality evidence;
- do not admit the lying/inconsistent record to the strict canonical stream.

### SourceSequenceConflictEvent

Same `SequenceDomain + SequenceEpoch + MmeSequenceNumber` contains distinct source identities after replay normalization.

**Current gap:** conflict is detected/logged but the durable contract is not built yet.

P1 target: publish `SourceSequenceConflictEvent` to `surv.dataquality.v1` with:

```text
SequenceDomain / SequenceEpoch / MmeSequenceNumber
both EventIds
both message identities
both Kafka coordinates
payload hashes
```

This is a critical source-data incident, not a fraud alert.

### CoverageGapEvent

A source sequence is absent only after the validated safe watermark proves all relevant active source progress beyond it.

Action:

- persist exact missing range;
- open degraded coverage epoch;
- retain proof used to declare the gap;
- continue according to replay/recovery policy.

A sparse jump inside one Kafka topic is never enough to declare a gap.

### SourceStalledEvent

One required source family/checkpoint stops progressing.

Action:

- do not classify future absent sequences as gaps without proof;
- mark coverage delayed/stalled;
- alert operations;
- apply bounded-buffer/backpressure behavior.

### UnknownDropMessageEvent - planned

Intended source: `mme.drop.parsed.unhandled`.

**Current status:** contract exists/planned, source-quality topic is not wired into the current Ingestor worker yet.

When wired:

- preserve valid source identity;
- count the source sequence as observed for continuity when appropriate;
- preserve forensic payload/evidence;
- mark semantic coverage unknown for that message.

### SourceParseFailureEvent - partially current / full raw-DLQ wiring planned

Normal adapter parse failures already produce durable data-quality evidence in the current Ingestor path.

The raw existing platform topic `mme.drop.raw.messages.dlq` is **not yet consumed** by the new Ingestor. When wired, preserve raw source identity/evidence so a source record cannot disappear merely because upstream normal parsing failed.

## Source topic availability

Topic inventory must not be treated as one binary “all 37 must exist” rule.

Classify each registry entry:

```text
Required
Optional
NotProvisioned
```

Behavior:

```text
Required missing        -> coverage degraded
Optional missing        -> warning/visibility
NotProvisioned missing  -> expected environment state
```

The 15 currently absent documented topics must be reconciled against the real environment before production coverage claims.

## Source-offset durability gap - P0

Current risk shape:

```text
consume record
→ buffer only in RAM
→ commit Kafka source offset
→ crash before canonical output
```

The event can then be permanently invisible to THE EYE after restart.

Target:

```text
consume
→ validate/buffer
→ durable terminal publication
→ mark source record safe
→ commit highest contiguous safe source offset per topic-partition
```

Do not commit a later source offset past an earlier unresolved record in the same Kafka partition.

Duplicate replay after crash is acceptable and is handled by deterministic `EventId`. Silent loss is not acceptable.

## Redis / watermark outage

If Redis watermark reads fail:

- retain the last trustworthy watermark if one exists;
- do not advance proof based on guesses;
- release contiguous already-known source records;
- stop at the first unresolved hole;
- never invent a `CoverageGapEvent`;
- expose buffer depth/stall/backpressure metrics.

## Reference quality events - Silo-side

```text
ReferenceNotReady
MissingOrderBookReference
MissingParticipantReference
MissingActorReference
MissingAccountReference
MissingInvestorReference
ReferenceActionConflict
ReferenceReplayMismatch
```

Rules requiring missing identity must not silently substitute `0`, empty values or today's Redis value.

Investor resolution is specifically:

```text
Order/Trade account
→ Account reference as-of source sequence
→ InvestorId
→ Investor state
```

## Trade quality events - Silo-side

```text
UnmatchedTradeSideTimeout
TradePairConflict
UnknownOrderReferencedByTrade
TradeLifecycleConflict
```

The Silo `TradePairProjector` builds deterministic matched-trade state from canonical trade sides.

## Order-book consistency events - Silo-side

```text
ModifyUnknownOrder
CancelUnknownOrder
ImpossibleQuantityTransition
DuplicateLifecycleTransition
NegativeRemainingQuantity
BboReconstructionMismatch
UnexpectedOrderStateTransition
```

These are structural/data-quality facts, not automatically fraud alerts.

## External data-domain availability

Each external domain has runtime state:

```text
NotConnected
ConnectedUnvalidated
ValidatedLive
ValidatedReplay
Degraded
Stale
```

## Rule evaluability

Each rule declares required/optional domains and acceptable feed/reference coverage.

Result states include:

```text
EvaluatedNoAlert
EvaluatedAlert
NotEvaluableMissingDomain
NotEvaluableCoverageGap
NotEvaluableReferenceGap
NotEvaluableStaleData
```

Example:

```text
Front Running
DROP orders/trades = healthy
ClientOrderInstruction = NotConnected
=> NotEvaluableMissingDomain
```

Never report “no front running detected” when the required client-order domain was absent.

## Coverage epoch

Conceptual record:

```text
CoverageEpoch
- EpochId
- SequenceDomain
- StartSourceSequence
- EndSourceSequence?
- StartTime
- EndTime?
- FeedStatus
- ParseStatus
- ReferenceStatus
- DomainStatuses
- GapIds[]
```

Alerts/rule-evaluation records point to the applicable coverage state.

## Recovery

When missing data is recovered:

```text
1. keep the original degraded epoch
2. replay/rebuild affected Silo state
3. reevaluate affected windows when policy requires
4. produce versioned recovered/recomputed results
5. never erase that live coverage was degraded at the time
```

## Operational metrics

Minimum source metrics:

```text
source_sequence_last_finalized
source_safe_watermark
source_buffer_depth
source_buffer_oldest_age
source_duplicate_count
source_conflict_count
source_gap_count
source_gap_width
source_metadata_mismatch_count
source_header_decode_failure_count
source_parse_failure_count
source_topic_required_missing_count
source_topic_optional_missing_count
source_commit_safe_lag
canonical_publish_lag
canonical_sequence_order_violation_count
```

Downstream metrics:

```text
reference_missing_count
rules_not_evaluable_by_reason
per-domain last event age
```

## Acceptance tests before production detector rollout

1. Replay duplicated source identities and prove one state application.
2. Remove one source sequence and prove a gap is declared only after safe watermark.
3. Delay one source family and prove no false gap.
4. Remove/corrupt source headers and prove quarantine with no invented values.
5. Corrupt message-id identity and prove mismatch handling.
6. Crash after buffer insert before canonical publish and prove source replay recovers the event.
7. Crash after canonical publish before source commit and prove replay causes no duplicate state effect.
8. Prove source Kafka commits advance only across contiguous durable records.
9. Disable Redis watermark reads and prove release stalls at an unresolved hole.
10. Replay reference update + old market event and prove as-of Account/Investor resolution.
11. Remove one trade side and prove the surviving side remains evidence.
12. Disable an external data domain and prove dependent rules become `NotEvaluableMissingDomain`.
13. Prove canonical source sequence is monotonic per `SequenceDomain`.

## Navigation

- [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[11 - External Event Contracts|External Event Contracts]]
- [[12 - Case Family Event Coverage Matrix|Case Family Event Coverage Matrix]]
- [[13 - Event Processing Blocks|Event Processing Blocks]]
