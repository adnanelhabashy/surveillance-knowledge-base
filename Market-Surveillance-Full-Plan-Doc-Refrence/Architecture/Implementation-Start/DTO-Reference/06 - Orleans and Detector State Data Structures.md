---
id: IMPL-DTO-06
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - orleans/state
  - detector/state
  - dotnet/domain
---

# Orleans and Detector State Data Structures

## Purpose

This page maps the mutable working-state structures implied by the active implementation architecture. These are **not source DTOs**. They belong in `TheEye.Domain` and/or the Orleans implementation project depending on whether they are pure reusable domain types or persistence-specific grain state.

## Ownership rule

```text
source DTOs/evidence -> Shared / TheEye.Contracts
pure value/state primitives -> TheEye.Domain
mutable grain-owned state -> TheEye.Silo
reusable detector calculations -> TheEye.Detectors
```

Do not serialize a grain implementation object directly as a public event contract.

## `OrderBookState`

**Recommended target:** `TheEye.Silo/State/OrderBookState.cs`  
**Owner:** `OrderBookGrain`

Starting state required by the active order-book design:

```text
ActiveOrdersById
BidLevels
AskLevels
BestBid
BestAsk
RecentExecutions
RecentAdds
RecentModifies
RecentCancels
PerTraderShortWindows
PerAccountShortWindows
RollingDepthSnapshots / imbalance summaries
LastAppliedSourceSequence
LastEventTime
CoverageEpoch reference
```

Rules:

- one mutable owner per live book;
- `LastAppliedSourceSequence` supports evidence/deduplication but does not imply `last + 1` for a book;
- all rolling collections are bounded;
- live state must be deterministically rebuildable from canonical replay.

---

## `ActiveOrderState`

**Recommended target:** `TheEye.Domain/Orders/ActiveOrderState.cs`

Minimum fields needed by the first detector set:

```text
OrderId
ParticipantId
ActorId
AccountId?
InvestorId?
Side
Price
OriginalQuantity
CurrentOrderQuantity
LeavesQuantity
DisplayedQuantity
ExecutedQuantity
CreatedAt
LastModifiedAt
OrderType
TimeInForce
OrderStatus
OrderBookPosition
LastSourceSequence
```

Keep only fields required for deterministic book/state/detector behavior. The complete raw `OrderLifecycleEvent` remains archived evidence and should not be duplicated blindly into state.

---

## `BookLevel`

**Recommended target:** `TheEye.Domain/OrderBook/BookLevel.cs`

Suggested state:

```text
Price
TotalVisibleQuantity
OrderCount
Ordered order references/IDs required to preserve book priority semantics
```

Exact queue/priority representation must follow the actual DROP lifecycle behavior and the first replay tests. Do not assume price-only aggregation is enough if order-level surveillance/priority evidence is required.

---

## `BookSnapshot`

**Recommended target:** `TheEye.Domain/OrderBook/BookSnapshot.cs`

Purpose: compact deterministic pre/post-event detector input.

Suggested information:

```text
BestBid / BestAsk
Top-N bid levels
Top-N ask levels
Visible bid depth
Visible ask depth
Book imbalance
SnapshotEventTime
SourceSequence
```

Do not copy the full book for every event unless profiling proves it necessary and affordable.

---

## `RecentExecution`

**Recommended target:** `TheEye.Domain/Trades/RecentExecution.cs`

Suggested fields:

```text
MatchId
OrderId
ParticipantId
ActorId
AccountId?
Side
Price
Quantity
EventTime
SourceSequence
SourceEventId
```

Use bounded recent windows for detector correlation. Full history remains in canonical/source evidence storage.

---

## `SubjectRollingWindow`

**Recommended target:** `TheEye.Domain/Windows/SubjectRollingWindow.cs`

Used for trader/account short-term behavior. It may be generic or implemented as specialized bounded counters.

Required concepts from the first detectors:

```text
100ms / 1s / 5s order-message counters
1s / 5s / 30s / 1m add/cancel/execute counters
recent execution window
recent completed order lifecycle window
size-distribution summary
levels touched
```

Prefer bounded ring buffers and rolling aggregates over unbounded `List<T>` collections.

---

## `DepthWindowSummary`

**Recommended target:** `TheEye.Domain/OrderBook/DepthWindowSummary.cs`

Supports multi-level pressure and displayed-size detectors:

```text
WindowStart
WindowEnd
Side
AddedVisibleQuantity
RemovedVisibleQuantity
LevelsTouched
MaxDepthShare
ImbalanceBefore
ImbalanceAfter
```

---

## `DetectorContext`

**Recommended target:** `TheEye.Detectors/Contracts/DetectorContext.cs`

The active detector spec defines the shared input concept:

```text
CurrentEvent
PreEventBookSnapshot
PostEventBookSnapshot
TraderWindow
AccountWindow?
InstrumentProfile
MarketPhase
CoverageState
EventTime
SourceSequence
```

Implementation recommendation: keep infrastructure objects out of this type. It should be constructible in a pure unit test.

---

## `CoverageState`

**Recommended target:** `Shared/Coverage/CoverageState.cs` if serialized between components, otherwise pure domain equivalent  
**Owner:** `CoverageStateGrain` / coverage projection

Required concepts:

```text
SequenceDomain
SequenceEpoch
CoverageEpochId
State = Healthy|Degraded|GapConfirmed|Recovering (exact enum is implementation choice)
Confirmed gaps/ranges or compact references
LastSafeWatermark
LastUpdated
```

The architecture requires coverage state to propagate into facts/alerts. Exact enum names beyond `NotEvaluableMissingDomain` are project implementation choices and should be frozen only after the source-assembly design is implemented.

---

## `DataDomainAvailability`

**Recommended target:** `Shared/Coverage/DataDomainAvailability.cs`

The external-data architecture explicitly requires domain availability values:

```text
NotConnected
ConnectedUnvalidated
ValidatedLive
ValidatedReplay
Degraded
```

Suggested structure:

```text
Domain
Status
LastValidatedAt?
LastReceivedAt?
Schema/AdapterVersion?
Reason?
```

---

## `EvaluabilityState`

**Recommended target:** `Shared/Coverage/EvaluabilityState.cs`

At minimum it must represent:

```text
Evaluable
NotEvaluableMissingDomain
```

Additional reasons may be added for coverage/data-quality degradation only after rule-evaluation semantics are explicitly designed. Do not silently interpret missing data as `false`.

---

## `ReferenceVersion<T>`

**Recommended target:** `TheEye.Domain/Reference/ReferenceVersion.cs`

Purpose: support as-of source-sequence resolution.

Suggested concepts:

```text
EntityKey
ValidFromSourceSequence
ValidToSourceSequence?
SourceEventId
Value
```

This is a project implementation structure derived from the active requirement that reference data be historically resolved as-of source sequence. The exact storage/index implementation may differ.

---

## `TradePairState`

**Recommended target:** `TheEye.Projections/TradePairState.cs`

Purpose: hold incomplete trade-side pairs keyed primarily by `matchId` until a deterministic `MatchedTradeEvent` can be produced.

Suggested concepts:

```text
MatchId
FirstSide
SecondSide?
FirstSeenSequence
LastUpdatedSequence
PairStatus
```

Bound/recover this state deterministically; do not let unmatched trades accumulate forever without a policy.

---

## `TransactionContextState`

**Recommended target:** `TheEye.Projections/TransactionContextState.cs`

Per DROP partition:

```text
PartitionId
CurrentTransactionId?
TransactionOpen
StartEventId?
StartSourceSequence?
```

`StartOfTransaction` opens context; `Commit` closes it. The native source DTOs remain unchanged.

---

## `BusinessDateContextState`

**Recommended target:** `TheEye.Projections/BusinessDateContextState.cs`

```text
CurrentBusinessDate?
SourceEventId
EffectiveSourceSequence
Initialized
```

Never use server calendar date as a silent fallback when source business date is unknown.

## State implementation constraints

- use deterministic data structures/comparers;
- bound rolling memory;
- avoid synchronous DB/network access on the grain event path;
- keep source evidence outside mutable state as durable canonical history;
- make snapshot persistence optional until recovery measurements justify it;
- avoid a grain per event or detector;
- start `OrderBookGrain` non-reentrant on the hot market path.

## Tests

```text
same ordered event stream -> same final state
replay duplicate -> no double application
sparse global sequence within one book -> no false gap
late reference version -> as-of lookup remains deterministic
rolling windows evict deterministically
book pre/post snapshots match lifecycle transition
state rebuild from replay equals live-applied state
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[03 - Derived Event Implementation Reference|Derived Events]]
- [[05 - Detector Fact Contract Reference|Detector Facts]]
- [[../03 - Order Book Surveillance Core|Order Book Surveillance Core]]
- [[../06 - First Detector Specifications|First Detector Specifications]]
- [[../10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[../13 - Event Processing Blocks|Event Processing Blocks]]
