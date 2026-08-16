---
id: ARCHETYPE-BOOK_PRESSURE
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Order-book pressure / quote manipulation

- **Catalog cases:** 25
- **Primary state owner:** `OrderBookGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required for core detection.

## Grain set

- `OrderBookGrain`
- `InstrumentGrain`
- `ParticipantInstrumentGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `OrderEvent`
- `ExecutionEvent`
- `BookSnapshot`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-018|CASE-018 — Odd-Lot Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-020|CASE-020 — Pegging Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-030|CASE-030 — Order-Book Imbalance Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-031|CASE-031 — Dominant Bid Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-032|CASE-032 — Dominant Offer Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-033|CASE-033 — Bid-Ask Spread Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-134|CASE-134 — Smoking]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-136|CASE-136 — Flying]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-211|CASE-211 — False Appearance of Customer Interest]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-214|CASE-214 — Pressure-to-Alter-Quote Scheme]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-218|CASE-218 — Non-Firm Stated-Price Offer]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-302|CASE-302 — Synthetic Depth Creation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-305|CASE-305 — Quote-Fade Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-306|CASE-306 — Bait-and-Switch Quoting]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-308|CASE-308 — Queue-Depletion Manipulation]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-313|CASE-313 — Odd-Lot Quote Shaping]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-319|CASE-319 — Locked-Market Inducement]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-320|CASE-320 — Crossed-Market Inducement]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-321|CASE-321 — Spread-Widening Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-322|CASE-322 — Spread-Narrowing Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-323|CASE-323 — One-Sided Book Pressure Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-324|CASE-324 — Book-Pressure Flip Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-325|CASE-325 — Order-Ladder Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-326|CASE-326 — Reserve-Order Replenishment Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-328|CASE-328 — Midpoint-Peg Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
