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

## Purpose

A surveillance system must distinguish:

```text
No suspicious behavior observed
from
Required data was missing / incomplete / not connected
```

This note defines that boundary.

## Coverage dimensions

Track at least four independent dimensions:

```text
FeedCoverage       -> did all expected source messages arrive?
ParseCoverage      -> could source messages be decoded/identified?
ReferenceCoverage  -> was required identity/instrument context available as-of the event?
DomainCoverage     -> were required external data domains connected and healthy?
```

A rule can be evaluable only when its mandatory coverage dimensions are acceptable.

## Source integrity events

### MissingSequenceHeader

A current source Kafka record lacks `mme-sequence-number`.

Action:

- preserve Kafka evidence;
- do not synthesize a source sequence;
- mark strict ordering/coverage degraded;
- route to source-quality investigation.

### SourceMetadataMismatchEvent

Kafka DROP headers disagree with payload message group/id/partition.

Action:

- retain both values;
- do not silently choose one;
- treat as source integrity issue.

### ConflictingSameSequencePayload

Same `SequenceDomain + SequenceEpoch + SourceSequence` resolves to conflicting source message identities/payloads after duplicate normalization.

Action: critical source-data incident.

### CoverageGapEvent

A source sequence is absent after the validated safe watermark proves source progress beyond it.

Action:

- persist missing range;
- open degraded coverage epoch;
- continue according to recovery policy;
- tag overlapping alerts.

### SourceStalledEvent

One required current ingestor health/checkpoint stops progressing.

Action:

- do not call future sequences missing yet;
- mark live coverage delayed/stalled;
- alert operations.

### UnknownDropMessageEvent

Source message reaches `mme.drop.parsed.unhandled`.

Action:

- count it as observed source sequence when identity is valid;
- preserve it in audit ledger;
- mark business semantic coverage unknown for that message.

### SourceParseFailureEvent

Raw source message reaches `mme.drop.raw.messages.dlq`.

Action:

- preserve source sequence/header/raw evidence if available;
- count source arrival separately from semantic parse success;
- open parse-degraded state if required for surveillance.

## Reference quality events

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

Rules requiring missing identity should not silently substitute `0`, empty string or today's latest value.

## Trade quality events

```text
UnmatchedTradeSideTimeout
TradePairConflict
UnknownOrderReferencedByTrade
TradeLifecycleConflict
```

Current enriched trade output is not used to hide these issues.

## Order-book consistency events

```text
ModifyUnknownOrder
CancelUnknownOrder
ImpossibleQuantityTransition
DuplicateLifecycleTransition
NegativeRemainingQuantity
BboReconstructionMismatch
UnexpectedOrderStateTransition
```

These are structural/data-quality signals. A rule may use them as supporting context, but they are not automatically fraud alerts.

## External data-domain availability

Every required external domain has runtime state:

```text
NotConnected
ConnectedUnvalidated
ValidatedLive
ValidatedReplay
Degraded
Stale
```

Examples:

```text
ClientOrders = NotConnected
BorrowLocate = ValidatedLive
Settlement = Stale
NewsPromotion = Degraded
```

Candidate rule routing uses this state.

## Rule evaluability

Each rule declares:

```text
RequiredDomains
OptionalDomains
MinimumFeedCoverage
MinimumReferenceCoverage
MaximumAllowedStaleness
```

Result states:

```text
EvaluatedNoAlert
EvaluatedAlert
NotEvaluableMissingDomain
NotEvaluableCoverageGap
NotEvaluableReferenceGap
NotEvaluableStaleData
```

This must be stored for coverage reporting.

## Example

```text
CASE: Front Running
Required:
- DROP Order/Trade = healthy
- ClientOrderInstruction = NotConnected

Result:
NotEvaluableMissingDomain
```

Not:

```text
No front running detected
```

## Coverage epoch

A compact coverage record:

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

Alerts and rule evaluation records point to the applicable coverage epoch.

## Recovery

When a missing sequence or external source is recovered:

```text
1. preserve original degraded epoch
2. replay/rebuild affected state
3. reevaluate affected rule windows if policy requires
4. create a recovered/recomputed result version
5. never erase the fact that live coverage was degraded at the time
```

## Operational metrics

Minimum metrics:

```text
source_sequence_last_finalized
source_safe_watermark
source_buffer_depth
source_duplicate_count
source_gap_count
source_gap_width
source_metadata_mismatch_count
source_parse_failure_count
reference_missing_count
global canonical lag
per-domain last event age
rules_not_evaluable_by_reason
```

## Acceptance tests before production detector rollout

1. Replay duplicated sequences and prove one state application.
2. Remove one source sequence and prove a gap is declared only after safe watermark.
3. Delay one family topic and prove no false gap before watermark.
4. Remove `mme-sequence-number` and prove the event is quarantined/degraded, not given a fake sequence.
5. Corrupt message-id header and prove mismatch detection.
6. Replay reference update + old market event and prove as-of reference resolution.
7. Remove one trade side and prove the surviving side remains evidence.
8. Disable an external data domain and prove dependent rules become `NotEvaluableMissingDomain`.

## Navigation

- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[11 - External Event Contracts|External Event Contracts]]
- [[12 - Case Family Event Coverage Matrix|Case Family Event Coverage Matrix]]
- [[13 - Event Processing Blocks|Event Processing Blocks]]
