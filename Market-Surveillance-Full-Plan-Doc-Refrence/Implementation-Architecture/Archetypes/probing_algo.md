---
id: ARCHETYPE-PROBING_ALGO
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Probing / algorithmic liquidity discovery abuse

- **Catalog cases:** 14
- **Primary state owner:** `OrderBookGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Optional later for adaptive/novel algorithm patterns; core scenarios are rules.

## Grain set

- `OrderBookGrain`
- `ParticipantInstrumentGrain`
- `InstrumentGrain`
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

- [[Cases/CASE-034|CASE-034 — Pinging / Liquidity Detection Abuse]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-135|CASE-135 — Phishing / Order-Book Phishing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-307|CASE-307 — Queue-Position Manipulation]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-310|CASE-310 — Minimum-Quantity Probing Abuse]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-311|CASE-311 — IOC Liquidity-Probing Abuse]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-312|CASE-312 — FOK Liquidity-Probing Abuse]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-480|CASE-480 — Selective Latency Advantage Abuse]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Cross-Venue Synchronization
- [[Cases/CASE-481|CASE-481 — Hidden Order-Priority Advantage]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Order-Message Burst Rate
- [[Cases/CASE-511|CASE-511 — Algorithm-Identifier Switching]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching, Order-Message Burst Rate, Order Lifetime
- [[Cases/CASE-531|CASE-531 — Algorithm Gaming]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-534|CASE-534 — Order-Anticipation Manipulation]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-538|CASE-538 — Order-Type Exploitation Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-539|CASE-539 — Hidden-Liquidity Algorithm Probing]] — detectors: Order-Message Burst Rate, Order Lifetime, Cross-Venue Synchronization
- [[Cases/CASE-540|CASE-540 — Cross-Venue Latency Manipulation]] — detectors: Cross-Venue Synchronization, Related-Account Graph, Time / Price / Quantity Matching

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
