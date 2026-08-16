---
id: ARCHETYPE-SPOOF_LAYER
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Spoofing / layering / deceptive liquidity

- **Catalog cases:** 26
- **Primary state owner:** `OrderBookGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required for core detection.

## Grain set

- `OrderBookGrain`
- `ParticipantInstrumentGrain`
- `TraderGrain`
- `AccountGrain`
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

- [[Cases/CASE-001|CASE-001 — Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-002|CASE-002 — Layering]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-028|CASE-028 — Phantom Liquidity]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-029|CASE-029 — Liquidity Mirage]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-123|CASE-123 — Spoof-and-Trade]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-124|CASE-124 — Layer-and-Trade]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-129|CASE-129 — Cross-Product Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-170|CASE-170 — Auction Indicative-Price Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-191|CASE-191 — NBBO Join-Inducement / Disruptive Quoting]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-237|CASE-237 — Neighboring Options-Series Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-293|CASE-293 — Inside-Spread Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-294|CASE-294 — Best-Bid Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-295|CASE-295 — Best-Offer Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-296|CASE-296 — Away-From-Touch Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-297|CASE-297 — Cancel-on-Touch Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-298|CASE-298 — Replenishing Spoof Orders]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-299|CASE-299 — Moving-Wall Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-300|CASE-300 — Fake Support Wall]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-301|CASE-301 — Fake Resistance Wall]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-315|CASE-315 — Micro-Order Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-316|CASE-316 — Microburst Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-327|CASE-327 — Iceberg-Spoof Combination]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-396|CASE-396 — Correlated-Basket Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-505|CASE-505 — Multi-Account Layering]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-506|CASE-506 — Multi-Broker Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure
- [[Cases/CASE-507|CASE-507 — Multi-Venue Spoofing]] — detectors: Cancellation Ratio, Order Lifetime, Displayed-Size Anomaly, Multi-Level Depth Pressure

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
