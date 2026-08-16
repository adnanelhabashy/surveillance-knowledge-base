---
id: ARCHETYPE-REPORTING
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Trade reporting / publication / identifier manipulation

- **Catalog cases:** 25
- **Primary state owner:** `TradeReportingGrain`
- **Primary services:** Trade Reporting Adapter, Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `TradeReportingGrain`
- `ParticipantInstrumentGrain`
- `AccountGrain`
- `RelationshipGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `TradeReportEvent`
- `IdentifierEvent`
- `MarketDataPublicationEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-113|CASE-113 — Trade-Reporting Manipulation]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-114|CASE-114 — Delayed-Trade Reporting Abuse]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-115|CASE-115 — False Price Reporting]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-116|CASE-116 — False Volume Reporting]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Trade-Report Timing / Accuracy
- [[Cases/CASE-137|CASE-137 — Printing]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-208|CASE-208 — Fictitious Quotation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-209|CASE-209 — False Transaction Publication]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-213|CASE-213 — Coordinated Trade-Reporting Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Trade-Report Timing / Accuracy
- [[Cases/CASE-280|CASE-280 — Execution-Venue Masking]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-488|CASE-488 — Duplicate Trade Reporting Manipulation]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-489|CASE-489 — Cancelled-Trade Publication Abuse]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-490|CASE-490 — False Trade Timestamp Reporting]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-491|CASE-491 — False Venue Reporting]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-492|CASE-492 — False Capacity Reporting]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-494|CASE-494 — False Account-Type Reporting]] — detectors: Displayed-Size Anomaly, Volume Participation, Liquidity Concentration
- [[Cases/CASE-495|CASE-495 — False Order-Origin Reporting]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-496|CASE-496 — False Cancel/Correct Report]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-497|CASE-497 — Broken-Trade Concealment]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-498|CASE-498 — Block-Size Misreporting]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-499|CASE-499 — Out-of-Sequence Print Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-500|CASE-500 — Off-Exchange Print Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Benchmark-Window Participation
- [[Cases/CASE-501|CASE-501 — Trade-Report Suppression]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-502|CASE-502 — Phantom Trade Print]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-503|CASE-503 — Pump-and-Dump Reporting Delay]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-504|CASE-504 — Average-Price Allocation Concealment]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact, Time / Price / Quantity Matching

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
