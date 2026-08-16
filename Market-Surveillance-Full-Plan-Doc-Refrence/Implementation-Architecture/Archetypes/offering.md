---
id: ARCHETYPE-OFFERING
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — IPO / offering / distribution / stabilization abuse

- **Catalog cases:** 16
- **Primary state owner:** `CorporateEventGrain`
- **Primary services:** Corporate/Offering Adapter, Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `CorporateEventGrain`
- `ParticipantInstrumentGrain`
- `PositionGrain`
- `InstrumentGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `OfferingEvent`
- `AllocationEvent`
- `ExecutionEvent`
- `OrderEvent`
- `PositionEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-021|CASE-021 — Price Capping]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-112|CASE-112 — IPO Allocation / Spinning Abuse]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-133|CASE-133 — Event-Driven Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-160|CASE-160 — Alternative Merger / Exchange-Offer Price Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-163|CASE-163 — IPO Allocation-for-Excessive-Commission Scheme]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-164|CASE-164 — Artificial Aftermarket Conditioning During a Distribution]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-183|CASE-183 — Fake Tender-Offer / Regulatory-Filing Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-189|CASE-189 — Improper Price Stabilization]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-200|CASE-200 — Distribution Restricted-Period Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-241|CASE-241 — IPO Flipping-Penalty Recoupment Abuse]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-244|CASE-244 — Returned IPO Share Premium Capture]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Volume Participation
- [[Cases/CASE-245|CASE-245 — Payment-for-Order-Flow Biased Routing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-256|CASE-256 — Tender-Offer Outside-Purchase / Unequal-Consideration Abuse]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-257|CASE-257 — Short Tendering]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-258|CASE-258 — Hedged Tendering]] — detectors: Pre-Event Abnormal Trading, Benchmark-Window Participation, Price Impact
- [[Cases/CASE-282|CASE-282 — Selective Advance Distribution of Research]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
