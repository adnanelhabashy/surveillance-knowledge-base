---
id: ARCHETYPE-ACCOUNT_SECURITY
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Account takeover / identity misuse

- **Catalog cases:** 10
- **Primary state owner:** `AccountSecurityGrain`
- **Primary services:** Account Security Adapter, Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Optional later for behavioral anomaly scoring; rule detection can operate without it.

## Grain set

- `AccountSecurityGrain`
- `AccountGrain`
- `ParticipantInstrumentGrain`
- `PositionGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `AccountSecurityEvent`
- `LoginEvent`
- `DeviceEvent`
- `ExecutionEvent`
- `TransferEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-100|CASE-100 — Unauthorized Trading]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-107|CASE-107 — Account-Takeover Trading]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-108|CASE-108 — Synthetic-Identity Trading Accounts]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-109|CASE-109 — Stolen-Identity Brokerage Accounts]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-110|CASE-110 — Money-Mule Trading Accounts]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-111|CASE-111 — Dormant-Account Takeover]] — detectors: Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-194|CASE-194 — Hacked-Account Forced-Buy Pump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Position Concentration
- [[Cases/CASE-196|CASE-196 — New-Account Identity Options Fraud]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Related-Account Graph
- [[Cases/CASE-204|CASE-204 — ACATS Account-Transfer Fraud]] — detectors: Cross-Venue Synchronization, Trade-Report Timing / Accuracy, Price Impact, Related-Account Graph
- [[Cases/CASE-205|CASE-205 — Digital-Signature Trading-Authorization Forgery]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
