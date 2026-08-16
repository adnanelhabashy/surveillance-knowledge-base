---
id: ARCHETYPE-EVASION
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Threshold / identity / surveillance-evasion behavior

- **Catalog cases:** 8
- **Primary state owner:** `RelationshipGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Not required; entity-resolution AI could enrich later.

## Grain set

- `RelationshipGrain`
- `AccountGrain`
- `TraderGrain`
- `ParticipantInstrumentGrain`
- `CoordinationWindowGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `OrderEvent`
- `ExecutionEvent`
- `IdentifierEvent`
- `RelationshipEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-118|CASE-118 — Order Splitting to Evade Surveillance]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-120|CASE-120 — Broker-Hopping / Venue-Hopping Evasion]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-138|CASE-138 — Trading-Safeguard Bypass Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-228|CASE-228 — Margin-Evasion Through Error Accounts]] — detectors: Related-Account Graph, Position Concentration, Time / Price / Quantity Matching, Trade-Report Timing / Accuracy
- [[Cases/CASE-510|CASE-510 — Trader-Identifier Switching]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-512|CASE-512 — Subaccount Cycling]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-513|CASE-513 — Temporal Threshold Evasion]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-514|CASE-514 — Cross-Security Threshold Evasion]] — detectors: Related-Account Graph, Position Concentration

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
