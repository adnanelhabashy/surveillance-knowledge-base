---
id: ARCHETYPE-CROSS_PRODUCT
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Cross-product / derivative / ETF / index manipulation

- **Catalog cases:** 40
- **Primary state owner:** `InstrumentRelationGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `InstrumentRelationGrain`
- `PositionGrain`
- `InstrumentGrain`
- `ParticipantInstrumentGrain`
- `BenchmarkWindowGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `OrderEvent`
- `PositionEvent`
- `InstrumentRelationEvent`
- `ReferencePriceEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-017|CASE-017 — Mini-Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-045|CASE-045 — Cross-Product Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-046|CASE-046 — Underlying-vs-Derivative Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-047|CASE-047 — Derivative-to-Stock Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-084|CASE-084 — Index-Level Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-131|CASE-131 — Portfolio / Basket Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-132|CASE-132 — Coordinated Multi-Security Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-172|CASE-172 — Pre-Expiry Related-Instrument Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-173|CASE-173 — Strike / Barrier Pinning]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-174|CASE-174 — Barrier-Trigger Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-176|CASE-176 — Artificial Arbitrage Creation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-192|CASE-192 — Toxic / Death-Spiral Convertible Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-195|CASE-195 — Account-Takeover Options Cross-Trade Fraud]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Time / Price / Quantity Matching
- [[Cases/CASE-201|CASE-201 — ETP Creation/Redemption Exploitation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-202|CASE-202 — ETP Portfolio-Composition Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-203|CASE-203 — Correlated ETP / Options Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-266|CASE-266 — Manufactured Credit Event]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-267|CASE-267 — Security-Based-Swap Payment/Delivery Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Short / Borrow / Settlement Status
- [[Cases/CASE-368|CASE-368 — ETF-Rebalance Close Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-384|CASE-384 — Stock-to-Option Marking]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-385|CASE-385 — Option-to-Stock Signaling Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-386|CASE-386 — ETF-to-Component Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-388|CASE-388 — ADR-to-Local-Share Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-389|CASE-389 — Dual-Listed Share Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-390|CASE-390 — Futures-to-Cash Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-391|CASE-391 — Cash-to-Futures Settlement Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-392|CASE-392 — Convertible-to-Underlying Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-393|CASE-393 — Warrant-to-Underlying Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-394|CASE-394 — Rights-to-Underlying Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-395|CASE-395 — Preferred-to-Common Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-402|CASE-402 — Cross-Border Price-Lead Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-403|CASE-403 — Pair-Trade Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-404|CASE-404 — Gamma-Squeeze Manipulation Scheme]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Position Concentration
- [[Cases/CASE-405|CASE-405 — Creation-Basket Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-406|CASE-406 — Redemption-Basket Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-437|CASE-437 — Synthetic-Short Concealment]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-520|CASE-520 — Position-Limit Evasion Through Derivatives]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-521|CASE-521 — Non-Bona-Fide EFP/EFRP Transaction]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-529|CASE-529 — Options Exercise-Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Cross-Product Economic Benefit
- [[Cases/CASE-530|CASE-530 — Pin-Risk Manipulation]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Pre-Event Abnormal Trading

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
