---
id: ARCHETYPE-COORDINATION
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Related-account / coordinated-group behavior

- **Catalog cases:** 55
- **Primary state owner:** `CoordinationWindowGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Optional later for discovering unknown clusters; known relationships remain deterministic.

## Grain set

- `CoordinationWindowGrain`
- `RelationshipGrain`
- `AccountGrain`
- `BeneficialOwnerGrain`
- `ParticipantInstrumentGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `OrderEvent`
- `RelationshipEvent`
- `AccountReference`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-008|CASE-008 — Painting the Tape]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-036|CASE-036 — Collusive Trading]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-037|CASE-037 — Related-Account Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-038|CASE-038 — Nominee-Account Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-039|CASE-039 — Trader–Investor Collusion]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-040|CASE-040 — Broker–Client Collusion]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-042|CASE-042 — Cross-Broker Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Cross-Venue Synchronization
- [[Cases/CASE-043|CASE-043 — Cross-Venue Manipulation]] — detectors: Cross-Venue Synchronization, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-044|CASE-044 — Cross-Market Manipulation]] — detectors: Cross-Venue Synchronization, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-052|CASE-052 — Bear Raid]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Self / Related Beneficial Owner
- [[Cases/CASE-091|CASE-091 — Beneficial-Ownership Concealment]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-092|CASE-092 — Share Parking]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-093|CASE-093 — Nominee Ownership Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-094|CASE-094 — Hidden Control of Multiple Accounts]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-117|CASE-117 — Order-Identity Concealment]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-119|CASE-119 — Multi-Account Threshold Evasion]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-121|CASE-121 — Coordinated Timing Evasion]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Position Concentration
- [[Cases/CASE-122|CASE-122 — Beneficial-Owner Evasion]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Position Concentration
- [[Cases/CASE-149|CASE-149 — Reverse/Forward Split Share-Concentration Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-155|CASE-155 — Coordinated Same-Issuer Deposit-and-Liquidation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Self / Related Beneficial Owner
- [[Cases/CASE-156|CASE-156 — Omnibus-Account Liquidation Abuse]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-157|CASE-157 — Master/Subaccount Anonymity Abuse]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Cross-Product Economic Benefit
- [[Cases/CASE-165|CASE-165 — Mark-Down → Accumulate → Mark-Up → Distribute Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Volume Participation
- [[Cases/CASE-168|CASE-168 — Synchronized Illiquid-Stock Trading]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-197|CASE-197 — Share-Journaling Fragmentation Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Position Concentration
- [[Cases/CASE-198|CASE-198 — Nested Omnibus Concealment]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Cross-Product Economic Benefit
- [[Cases/CASE-212|CASE-212 — Quote Coordination Between Market Participants]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-215|CASE-215 — Market-Maker Retaliation / Intimidation]] — detectors: Displayed-Size Anomaly, Volume Participation, Liquidity Concentration
- [[Cases/CASE-262|CASE-262 — Derivative-Based Beneficial-Ownership Evasion]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-263|CASE-263 — Intentional Schedule 13D Ownership Concealment]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-264|CASE-264 — Hidden Beneficial-Ownership Group]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-265|CASE-265 — Late Beneficial-Ownership Filing After Threshold Crossing]] — detectors: Related-Account Graph, Position Concentration
- [[Cases/CASE-303|CASE-303 — Depth Withdrawal Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-309|CASE-309 — Order-Priority Gaming with Related Accounts]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Order-Message Burst Rate
- [[Cases/CASE-335|CASE-335 — Alternating Print Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Self / Related Beneficial Owner
- [[Cases/CASE-336|CASE-336 — Volume-Pulse Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-339|CASE-339 — Uneconomic Value-Transfer Trading]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-376|CASE-376 — Closing-Cross Participation Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Self / Related Beneficial Owner
- [[Cases/CASE-452|CASE-452 — Coordinated Limit-Order Pump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Self / Related Beneficial Owner
- [[Cases/CASE-456|CASE-456 — Nominee-Account Pump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Self / Related Beneficial Owner
- [[Cases/CASE-462|CASE-462 — Multi-Broker Dumping]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Self / Related Beneficial Owner
- [[Cases/CASE-465|CASE-465 — Nominee-Funnel-to-Omnibus Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-472|CASE-472 — Coordinated Quote Widening]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-473|CASE-473 — Coordinated Quote Narrowing]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-474|CASE-474 — Market-Maker Quote Synchronization]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-475|CASE-475 — Fake Two-Sided Market]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-476|CASE-476 — Skewed Two-Sided Quote Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-508|CASE-508 — Beneficial-Owner Cross-Broker Self-Trade]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Cross-Venue Synchronization
- [[Cases/CASE-509|CASE-509 — Nominee Account Rotation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-515|CASE-515 — Common-Funding Coordinated Trading]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-516|CASE-516 — Common-Withdrawal Coordinated Trading]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-517|CASE-517 — Synchronized-Access Trading Scheme]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-518|CASE-518 — Related-Account Order Relay]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-519|CASE-519 — Position-Limit Evasion Through Related Accounts]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Position Concentration
- [[Cases/CASE-535|CASE-535 — Self-Trade-Prevention Gaming]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
