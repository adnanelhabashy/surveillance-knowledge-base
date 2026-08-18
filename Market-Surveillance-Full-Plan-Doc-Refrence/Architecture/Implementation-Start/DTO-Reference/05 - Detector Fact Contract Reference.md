---
id: IMPL-DTO-05
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/detectors
  - dotnet/contracts
---

# Detector Fact Contract Reference

## Purpose

Detector facts are immutable measurements produced by deterministic .NET detector classes. They are not source events and they are not final fraud conclusions.

Default target:

```text
Shared/Facts/
```

Detectors receive explicit state/context and emit fact contracts with no Kafka, database or network dependency.

## Common fact metadata

Every fact should carry or inherit enough metadata for reproducibility:

```text
DetectorId
DetectorVersion
VenueId
InstrumentId
SubjectIds
WindowStart
WindowEnd
SourceSequenceMin
SourceSequenceMax
CoverageEpoch
CoverageDegraded
```

The exact inheritance/composition style is a .NET implementation choice. Prefer composition if it keeps serialized contracts clear.

## `OrderLifetimeFact`

**Target:** `Facts/OrderLifetimeFact.cs`  
**Detector:** `OrderLifetimeDetector`

```text
OrderId
LifetimeMs
WasCancelled
FillRatio
EntryDistanceFromTouchTicks
MaxDisplayedQuantity
+ common metadata
```

Very short lifetime is a measurement, not automatically suspicious.

---

## `CancellationRatioFact`

**Target:** `Facts/CancellationRatioFact.cs`  
**Detector:** `CancellationRatioDetector`

```text
AddedQuantity
CancelledQuantity
ExecutedQuantity
CancelledQtyRatio
CancelledOrderRatio
Window
+ common metadata
```

The detector uses rolling counters by subject/instrument over configured windows.

---

## `DisplayedSizeAnomalyFact`

**Target:** `Facts/DisplayedSizeAnomalyFact.cs`

```text
DisplayedQuantity
QuantityVsInstrumentMedian
QuantityPercentile
ShareOfVisibleDepth
DistanceFromTouchTicks
+ common metadata
```

Prefer percentile/rank features over a fixed global quantity threshold.

---

## `MultiLevelDepthPressureFact`

**Target:** `Facts/MultiLevelDepthPressureFact.cs`

```text
Side
LevelsUsed
AddedVisibleQuantity
RemovedVisibleQuantity
MaxDepthShare
BookImbalanceBefore
BookImbalanceAfter
DistanceRangeFromTouchTicks
+ common metadata
```

Separate continuous-trading and auction calibration profiles.

---

## `OppositeSideExecutionFact`

**Target:** `Facts/OppositeSideExecutionFact.cs`

```text
PressureSide
ExecutedSide
ExecutedQuantity
ExecutedValue
TimeFromPressureStartMs
TimeFromPressureRemovalMs
+ common metadata
```

This fact becomes meaningful when combined with pressure/cancel/low-fill evidence.

---

## `PriceImpactFact`

**Target:** `Facts/PriceImpactFact.cs`

```text
ReferencePriceBefore
MaxMoveTicks
SignedMoveTicks
MoveBps
ReversionTicks
ObservationWindowMs
+ common metadata
```

Normalize interpretation by tick size, spread, volatility and liquidity.

---

## `OrderMessageBurstFact`

**Target:** `Facts/OrderMessageBurstFact.cs`

```text
NewCount
ModifyCount
CancelCount
TotalMessageRate
DistinctOrders
DistinctLevels
WindowMs
+ common metadata
```

Use bounded counters/ring buffers rather than unbounded event lists.

---

## `OrderBookPressureFact`

**Target:** `Facts/OrderBookPressureFact.cs`  
**Status:** reusable aggregate fact proposed directly by the active Order Book Surveillance Core

```text
VenueId
InstrumentId
TraderId
Side
WindowStart
WindowEnd
AddedVisibleQuantity
CancelledQuantity
ExecutedQuantity
CancellationRatio
MaxDepthShare
LevelsUsed
BookImbalanceBefore
BookImbalanceAfter
PriceMoveTicks
OppositeSideExecutedQuantity
SourceSequenceMin
SourceSequenceMax
CoverageEpoch
```

This may be produced as a composed/aggregate fact after the first individual detector facts exist.

---

## `FactBundle`

**Target:** `Facts/FactBundle.cs`  
**Role:** immutable transport between reusable detectors and the Candidate Rule Router

Recommended starting structure:

```text
FactBundleId
EventTime / WindowEnd
VenueId
InstrumentId
SubjectIds
FactType keys
Facts[]
CoverageState
Source evidence summary
DetectorVersions
```

The active design requires a `FactBundle`; the exact generic/polymorphic serialization strategy is an implementation decision. Keep it explicit and versionable rather than passing arbitrary dictionaries into RulesEngine.

---

## `SpoofLayerFactBundle`

**Target:** `Facts/SpoofLayerFactBundle.cs`  
**Status:** first-slice typed convenience contract

Starting composition:

```text
OrderLifetimeFact
CancellationRatioFact
DisplayedSizeAnomalyFact
MultiLevelDepthPressureFact
OppositeSideExecutionFact?
PriceImpactFact?
OrderMessageBurstFact?
CoverageState
```

Not every optional fact must exist for every candidate; routing/evaluability decides which rule variants can run.

## Future fact contracts

The architecture names further reusable detector outputs that can be added incrementally, including concepts around:

```text
BookConsistency
TimePriceQuantityMatch
SelfRelatedOwner
AuctionImpact
VolumeParticipation
PositionConcentration
RelationshipCoordination
FailToDeliver
```

Do not create hundreds of case-specific fact DTOs upfront. Add reusable facts as detector families are implemented and validated against case coverage.

## Serialization rule

Facts must be:

```text
immutable
versionable
side-effect free
small enough for hot-path use
explicitly typed
replay reproducible
coverage aware
```

Avoid serializing grain implementation internals directly as facts.

## Tests

Each fact contract/detector pair needs:

```text
positive measurement fixture
normal/negative fixture
boundary fixture
coverage-degraded fixture
same input -> same fact values
serialization round trip
no infrastructure dependency in unit test
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[03 - Derived Event Implementation Reference|Derived Events]]
- [[06 - Orleans and Detector State Data Structures|State Data Structures]]
- [[../06 - First Detector Specifications|First Detector Specifications]]
- [[../03 - Order Book Surveillance Core|Order Book Surveillance Core]]
- [[../../../MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
