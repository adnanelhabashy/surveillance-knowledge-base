---
id: ARCHETYPE-ROUTING_VENUE
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Routing / execution quality / ATS / venue conflict

- **Catalog cases:** 23
- **Primary state owner:** `RoutingQualityGrain`
- **Primary services:** Client Order Adapter, Venue/Fees Adapter, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `RoutingQualityGrain`
- `ClientOrderWindowGrain`
- `ParticipantInstrumentGrain`
- `VenueGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ClientOrderEvent`
- `RoutingDecisionEvent`
- `ExecutionEvent`
- `VenueFeeEvent`
- `NBBOEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-103|CASE-103 — Best-Execution Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-104|CASE-104 — Interpositioning Abuse]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-246|CASE-246 — Maker-Taker Rebate Biased Routing]] — detectors: Displayed-Size Anomaly, Volume Participation, Liquidity Concentration
- [[Cases/CASE-247|CASE-247 — Affiliated-Venue Routing Conflict]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Displayed-Size Anomaly
- [[Cases/CASE-269|CASE-269 — Retail Execution-Quality Misrepresentation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-270|CASE-270 — Internalization Profit at Customer Expense]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-272|CASE-272 — Dark-Pool Subscriber Information Trading Abuse]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Pre-Event Abnormal Trading
- [[Cases/CASE-273|CASE-273 — Dark-Pool Subscriber Information Marketing Abuse]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-274|CASE-274 — Secret Proprietary Desk Inside an ATS]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Cross-Venue Synchronization
- [[Cases/CASE-275|CASE-275 — Hidden Affiliate Counterparty Dominance]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Cross-Venue Synchronization
- [[Cases/CASE-276|CASE-276 — False Dark-Pool Anti-Predatory-Trading Claims]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Cross-Venue Synchronization
- [[Cases/CASE-277|CASE-277 — Undisclosed Preferential ATS Order Type]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Cross-Venue Synchronization
- [[Cases/CASE-278|CASE-278 — Sub-Penny Priority Jumping]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact, Volume Participation
- [[Cases/CASE-281|CASE-281 — Selective Early Market-Data Dissemination]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-397|CASE-397 — Lit-to-Dark Manipulation]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-398|CASE-398 — Dark-to-Lit Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-426|CASE-426 — Misuse of Dark-Pool Order Information]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact, Cross-Venue Synchronization
- [[Cases/CASE-471|CASE-471 — Liquidity-Program Gaming]] — detectors: Displayed-Size Anomaly, Volume Participation, Liquidity Concentration, Order-Message Burst Rate
- [[Cases/CASE-482|CASE-482 — Broker Crossing-Engine Favoritism]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-483|CASE-483 — Affiliate Internalization Favoritism]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Cross-Venue Synchronization
- [[Cases/CASE-484|CASE-484 — Internalizer Price-Shading Abuse]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-485|CASE-485 — Trade-Through Concealment]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-486|CASE-486 — Stale-Quote Creation and Exploitation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
