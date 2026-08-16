---
id: ARCHETYPE-MESSAGE_ABUSE
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Message-rate / stuffing / burst abuse

- **Catalog cases:** 10
- **Primary state owner:** `ParticipantInstrumentGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `ParticipantInstrumentGrain`
- `OrderBookGrain`
- `TraderGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `OrderEvent`
- `OrderAckEvent`
- `MarketPhaseEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-024|CASE-024 — Quote Stuffing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-025|CASE-025 — Order Stuffing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-026|CASE-026 — Excessive Order Cancellation Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-027|CASE-027 — Flickering Orders]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-238|CASE-238 — Trade Shredding]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-317|CASE-317 — Cross-Symbol Quote Stuffing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-318|CASE-318 — Feed-Delay Quote Stuffing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-533|CASE-533 — Microburst Momentum Ignition]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Order-Message Burst Rate
- [[Cases/CASE-536|CASE-536 — Matching-Engine Stress Manipulation]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-537|CASE-537 — Cancel-Reenter Queue Monopolization]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
