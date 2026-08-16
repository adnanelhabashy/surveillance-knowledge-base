---
id: ARCHETYPE-SHORT_SETTLEMENT
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Short sale / borrow / settlement abuse

- **Catalog cases:** 29
- **Primary state owner:** `ShortSettlementGrain`
- **Primary services:** Short/Settlement Adapter, Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `ShortSettlementGrain`
- `PositionGrain`
- `ParticipantInstrumentGrain`
- `AccountGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ShortSaleEvent`
- `BorrowLocateEvent`
- `SettlementEvent`
- `ExecutionEvent`
- `PositionEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-095|CASE-095 — Manipulative Short Selling]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Short / Borrow / Settlement Status
- [[Cases/CASE-096|CASE-096 — Short-Sale Mismarking]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-097|CASE-097 — Locate / Borrow Misrepresentation]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-098|CASE-098 — Abusive Naked Short-Selling Scheme]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-102|CASE-102 — Trade Allocation Fraud / Cherry Picking]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact, Time / Price / Quantity Matching
- [[Cases/CASE-161|CASE-161 — Pre-Offering Short-Sale / Rule 105 Abuse]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-162|CASE-162 — IPO Aftermarket Tie-In Scheme]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-232|CASE-232 — Sham Married-Put Short-Sale Evasion]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-233|CASE-233 — Buy-Write Fail-to-Deliver Reset Scheme]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-234|CASE-234 — Stock-Kiting to Maintain Naked Short Position]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-253|CASE-253 — Improper Short-Exempt Marking]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-254|CASE-254 — Short-Sale Circuit-Breaker Exemption Abuse]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-433|CASE-433 — Long-Sale Mismarking]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-434|CASE-434 — Sham Locate]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-435|CASE-435 — Locate Reuse Abuse]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-436|CASE-436 — Hard-to-Borrow Locate Fraud]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-438|CASE-438 — Fail-to-Deliver Concealment]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-439|CASE-439 — Sham Close-Out Transaction]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-440|CASE-440 — Threshold-Security Close-Out Evasion]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-441|CASE-441 — Chronic Fail Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-442|CASE-442 — Stock-Borrow Squeeze Manipulation]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-446|CASE-446 — Recall-Timing Manipulation]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-447|CASE-447 — Locate-Availability False Signaling]] — detectors: Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-448|CASE-448 — Borrow-Inventory Corner]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-449|CASE-449 — Short-Squeeze Ignition]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-450|CASE-450 — Ex-Clearing Settlement Evasion]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-493|CASE-493 — False Short-Sale Indicator Reporting]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-524|CASE-524 — Delivery-Squeeze Manipulation]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-525|CASE-525 — Forced-Buy-In Manipulation]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration, Price Impact

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
