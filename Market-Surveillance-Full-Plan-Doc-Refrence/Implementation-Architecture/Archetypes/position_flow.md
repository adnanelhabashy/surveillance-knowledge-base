---
id: ARCHETYPE-POSITION_FLOW
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Corner / squeeze / concentration / inventory flow

- **Catalog cases:** 12
- **Primary state owner:** `PositionGrain`
- **Primary services:** Position/Holdings Adapter, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `PositionGrain`
- `InstrumentGrain`
- `BeneficialOwnerGrain`
- `ParticipantInstrumentGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `PositionEvent`
- `HoldingEvent`
- `ExecutionEvent`
- `FloatReferenceEvent`
- `BorrowLocateEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-054|CASE-054 — Low-Float Manipulation]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-088|CASE-088 — Cornering the Market]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-089|CASE-089 — Market Squeeze]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-090|CASE-090 — Float Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-147|CASE-147 — Restrictive-Legend Removal Abuse]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-154|CASE-154 — Deposit–Sell–Wire-Out Scheme]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-158|CASE-158 — Securities-Based Currency Conversion Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-159|CASE-159 — Mirror Trading Through Securities]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-169|CASE-169 — Fake Share-Provenance / Legal-Opinion Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-206|CASE-206 — ACH Instant-Funds Securities Abuse]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-219|CASE-219 — Broker Inventory Parking to Evade Capital Requirements]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-259|CASE-259 — Improper ADR Pre-Release]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
