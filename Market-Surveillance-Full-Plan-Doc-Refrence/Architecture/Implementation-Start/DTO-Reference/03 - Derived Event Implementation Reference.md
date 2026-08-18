---
id: IMPL-DTO-03
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/derived-events
  - dotnet/contracts
---

# Derived Event Implementation Reference

## Purpose

Derived events are deterministic THE EYE outputs built from canonical source events and projections. They are **not additional DROP messages**. Every derived contract must retain source lineage.

Default target folder:

```text
Shared/Events/Derived/
```

Default evidence field:

```text
DerivedEventEvidence Evidence
```

See [[01 - Core Envelopes and Evidence Reference|Core Envelopes and Evidence Reference]].

## `MatchedTradeEvent`

**Target:** `Events/Derived/MatchedTradeEvent.cs`  
**Built from:** compatible `TradeSideEvent` records sharing `matchId`  
**Producer:** `TradePairProjector`

Required implementation information:

```text
MatchId
OrderBookId
TradePrice
Quantity
TradeTime / resolved canonical event time
DealSource

Buy source event identity
Buy order / participant / actor / account / token identifiers

Sell source event identity
Sell order / participant / actor / account / token identifiers

Evidence
```

Validation rules:

- never discard either original `TradeSideEvent`;
- pair only when sides/context are compatible;
- duplicate/replayed sides must not create duplicate matched events;
- a correction/cancel trade lifecycle must remain representable;
- deterministic input must produce deterministic output ID/content.

Links: [[../13 - Event Processing Blocks|TradePairProjector]], [[02 - DROP Source DTO Implementation Reference|TradeSideEvent]].

---

## `ResolvedOrderEvent`

**Target:** `Events/Derived/ResolvedOrderEvent.cs`  
**Built from:** `OrderLifecycleEvent` + reference state as-of source sequence  
**Producer:** reference/context projection layer

Required information:

```text
SourceOrderEventId
SourceSequence
OrderBookId
Asset/Instrument identity
ParticipantId + resolved participant context
ActorId + resolved actor context
Account identity + resolved account context
Investor identity + resolved investor context
Custodian/account type/group context when applicable
Original OrderLifecycleEvent reference/evidence
```

Rule: never overwrite source numeric/string identifiers with resolved names. Resolution is additive and historical as-of the source sequence.

---

## `ResolvedTradeEvent`

**Target:** `Events/Derived/ResolvedTradeEvent.cs`  
**Built from:** `TradeSideEvent` or `MatchedTradeEvent` + as-of reference state

Required information mirrors `ResolvedOrderEvent` for trade identities and instrument context while retaining source-side/match lineage.

---

## `OrderBookStateChangedEvent`

**Target:** `Events/Derived/OrderBookStateChangedEvent.cs`  
**Built from:** canonical order/trade/BBO inputs applied to deterministic order-book state

Recommended implementation fields based on the active order-book architecture:

```text
VenueId
OrderBookId
InstrumentId
EventTime
SourceSequence
ChangeType
BestBidBefore / BestAskBefore
BestBidAfter / BestAskAfter
Depth/imbalance summary before and after when calculated
AffectedOrderId?
AffectedPriceLevel?
Evidence
```

The exact compact snapshot shape is an implementation choice. Do not copy the entire live book into every event.

---

## `AuctionStateEvent`

**Target:** `Events/Derived/AuctionStateEvent.cs`  
**Built from:** `SessionChangeEvent` + `EquilibriumPriceEvent` + relevant orders/trades/BBO

Minimum useful information:

```text
OrderBookId
InstrumentId
Auction/session phase
Indicative/equilibrium price
Bid quantity / offer quantity
Imbalance quantity/side when available
Session/event time
Evidence
```

Keep source session/equilibrium messages addressable individually.

---

## `PositionStateChangedEvent`

**Target:** `Events/Derived/PositionStateChangedEvent.cs`  
**Built from:** `AccountPositionEvent` plus optional deterministic execution-derived deltas

Required information:

```text
Asset/InstrumentId
ParticipantId
AccountId/account identity
InvestorId
PreviousAvailableLongQty?
AvailableLongQty
PreviousAvailableLoanQty?
AvailableLoanQty
Change source
Evidence
```

Do not reinterpret `AccountPositionEvent` as a full settlement ledger. Settlement obligation history is a separate external domain.

---

## `CoverageGapEvent`

**Target:** `Events/Derived/CoverageGapEvent.cs`  
**Producer:** global source assembly/coverage tracker

Required information:

```text
SequenceDomain
SequenceEpoch
MissingSequenceStart
MissingSequenceEnd
DetectedAt
WatermarkEvidence
CoverageEpochId
Affected source/domain classification
```

A gap is declared only by the global assembly layer after safe-watermark logic. Never detect a gap from sparse per-topic sequence values.

---

## `SourceMetadataMismatchEvent`

**Target:** `Events/Derived/SourceMetadataMismatchEvent.cs`

Use when Kafka DROP identity headers disagree with the payload.

Required information:

```text
Observed Kafka coordinates
MmeSequenceNumber when available
Expected payload MessageGroup/MessageId/PartitionId
Observed header MessageGroup/MessageId/PartitionId
Source event/raw reference
DetectedAt
```

Do not silently choose one side and continue as if the disagreement did not happen.

---

## `UnknownDropMessageEvent`

**Target:** `Events/Derived/UnknownDropMessageEvent.cs`  
**Input:** `mme.drop.parsed.unhandled`

Required information:

```text
Source identity available from headers/payload
MessageGroup/MessageId/PartitionId when available
MmeSequenceNumber when available
Kafka evidence
Raw/unhandled representation or stable reference/hash
Reason/classification
```

Purpose: keep the global source sequence explainable even when the application does not know the DTO.

---

## `SourceParseFailureEvent`

**Target:** `Events/Derived/SourceParseFailureEvent.cs`  
**Input:** `mme.drop.raw.messages.dlq` when source-sequence evidence is available

Required information:

```text
MmeSequenceNumber when available
DROP source identity headers when available
Kafka evidence
Failure classification/message
Raw record reference/hash or governed payload copy
DetectedAt
```

A parse failure is data-quality evidence, not fraud evidence.

---

## `BookConsistencyIssue`

**Target:** `Events/Derived/BookConsistencyIssue.cs`  
**Producer:** order-book consistency validation

Suggested structure from the active design:

```text
IssueType
VenueId
OrderBookId
InstrumentId
OrderId?
EventTime
SourceSequence
Description / structured details
Severity for data quality
Evidence
```

Issue types should cover examples such as unknown-order modify/cancel, impossible quantity transitions, duplicate lifecycle transitions and reconstructed-depth contradictions. It must never automatically be labeled manipulation.

---

## `SurveillanceAlertEvent`

**Target:** `Events/Derived/SurveillanceAlertEvent.cs`  
**Producer:** rules + alert correlation/evidence builder

Minimum contract from the first vertical slice:

```text
AlertId
CaseId
RuleVersion
DetectorVersion(s)
ThresholdProfileVersion
SubjectIds
VenueId / OrderBookId / InstrumentId
WindowStart / WindowEnd
Score / Severity
SourceEventIds
MME sequence range/list
Kafka evidence coordinates
CoverageEpoch / CoverageDegraded
Data-domain availability/evaluability
Evidence summary
ReplayRunId?
```

Alerts must be reproducible from stored source evidence and versions.

---

## Fact output boundary

`FactBundle` is intentionally documented under [[05 - Detector Fact Contract Reference|Detector Fact Contract Reference]] rather than treating detector facts as source events.

## Derived ID rule

Derived output IDs should be deterministic from stable input evidence + producer/version identity where replay reproducibility requires deduplication. Do not use an arbitrary GUID as the only identity for replayable derived output.

## Tests required

```text
same canonical input -> same derived output
source lineage survives serialization
replayed duplicate source does not duplicate derived state/output
coverage degraded flag propagates
reference resolution is correct as-of source sequence
trade pairing handles out-of-order compatible sides deterministically
invalid/ambiguous input produces quality state rather than fabricated data
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[01 - Core Envelopes and Evidence Reference|Core Envelopes + Evidence]]
- [[02 - DROP Source DTO Implementation Reference|DROP Source DTOs]]
- [[05 - Detector Fact Contract Reference|Detector Facts]]
- [[06 - Orleans and Detector State Data Structures|State Data Structures]]
- [[../03 - Order Book Surveillance Core|Order Book Surveillance Core]]
- [[../13 - Event Processing Blocks|Event Processing Blocks]]
