---
id: ARCHETYPE-BENCHMARK
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Benchmark / VWAP / TWAP / settlement reference abuse

- **Catalog cases:** 30
- **Primary state owner:** `BenchmarkWindowGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `BenchmarkWindowGrain`
- `InstrumentGrain`
- `ParticipantInstrumentGrain`
- `PositionGrain`
- `InstrumentRelationGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `BenchmarkEvent`
- `PositionEvent`
- `ReferencePriceEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-019|CASE-019 — Reference-Price Gaming]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Order-Message Burst Rate
- [[Cases/CASE-081|CASE-081 — VWAP Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-082|CASE-082 — TWAP Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-083|CASE-083 — Settlement-Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Benchmark-Window Participation
- [[Cases/CASE-085|CASE-085 — NAV Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-086|CASE-086 — Benchmark Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-087|CASE-087 — Reference-Rate Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-175|CASE-175 — Margin-Settlement Reference Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-177|CASE-177 — Rights-Issue Reference-Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Cross-Product Economic Benefit
- [[Cases/CASE-185|CASE-185 — Mutual-Fund Late Trading]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-220|CASE-220 — Mark-to-Market Inflation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-221|CASE-221 — Investment-Fund Asset Overvaluation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-223|CASE-223 — Fraudulent NAV Pricing]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-268|CASE-268 — Security-Based-Swap Valuation Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-279|CASE-279 — Selective ATS Functionality Access]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Cross-Venue Synchronization
- [[Cases/CASE-329|CASE-329 — Pegged-Order Reference Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-358|CASE-358 — VWAP-Window Concentration Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-359|CASE-359 — TWAP-Window Concentration Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-360|CASE-360 — Off-Market Cross Price Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-361|CASE-361 — Negotiated-Cross Price Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-364|CASE-364 — End-of-Month Marking]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-378|CASE-378 — Settlement-Window Ramping]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-380|CASE-380 — Fixing-Window Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-381|CASE-381 — Benchmark-Window Concentration]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-382|CASE-382 — Index-Calculation Constituent Marking]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-383|CASE-383 — NAV-Strike Price Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-387|CASE-387 — Component-to-ETF Manipulation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-399|CASE-399 — Dark-Pool Reference-Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Benchmark-Window Participation
- [[Cases/CASE-407|CASE-407 — Rebalance-Price-Impact Exaggeration]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-487|CASE-487 — Midpoint Reference Manipulation by Venue User]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
