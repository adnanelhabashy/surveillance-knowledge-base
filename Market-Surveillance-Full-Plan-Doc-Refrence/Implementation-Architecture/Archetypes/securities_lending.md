---
id: ARCHETYPE-SECURITIES_LENDING
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Securities lending abuse

- **Catalog cases:** 13
- **Primary state owner:** `SecuritiesLoanGrain`
- **Primary services:** Securities Lending Adapter, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `SecuritiesLoanGrain`
- `PositionGrain`
- `AccountGrain`
- `BeneficialOwnerGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `SecuritiesLoanEvent`
- `CollateralEvent`
- `SettlementEvent`
- `AccountReference`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-235|CASE-235 — Stock-Loan Sham Finder-Fee Scheme]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-236|CASE-236 — Stock-Loan Kickback Scheme]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-251|CASE-251 — Fully Paid Securities Lending Without Proper Consent or Disclosure]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-252|CASE-252 — Unsuitable Fully Paid Securities Lending]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-286|CASE-286 — False Securities-Loan Reporting]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-287|CASE-287 — Securities-Loan Quantity Misreporting]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-288|CASE-288 — Securities-Loan Rate / Fee Misreporting]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-289|CASE-289 — Securities-Loan Modification Concealment]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-290|CASE-290 — Undercollateralized Customer Securities Borrowing]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-291|CASE-291 — Customer Fully-Paid Securities Possession/Control Abuse]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-443|CASE-443 — Borrow-Fee Manipulation]] — detectors: Short / Borrow / Settlement Status, Position Concentration, Liquidity Concentration
- [[Cases/CASE-444|CASE-444 — Securities-Lending Wash Transaction]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Short / Borrow / Settlement Status
- [[Cases/CASE-445|CASE-445 — Matched Stock-Loan Transactions]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Short / Borrow / Settlement Status

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
