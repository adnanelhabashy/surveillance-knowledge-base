---
id: CASE-286
type: surveillance-case
case_number: 286
title: "False Securities-Loan Reporting"
status: implementation-seeded
implementation_archetype: securities_lending
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 286. False Securities-Loan Reporting

## Description

A securities borrower, lender, or reporting intermediary submits fictitious or deliberately false securities-loan information to the regulatory reporting system.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-289|Securities-Loan Modification Concealment]]
- [[Cases/CASE-288|Securities-Loan Rate / Fee Misreporting]]
- [[Cases/CASE-078|False Corporate Disclosure]]
- [[Cases/CASE-219|Broker Inventory Parking to Evade Capital Requirements]]
- [[Cases/CASE-491|False Venue Reporting]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + securities-lending data; AI not required
- **Rule logic (starter):** Flag securities-lending transactions with inconsistent ownership, collateral, quantity/rate reporting, unusual related-party economics, sham finder fees/kickbacks, or unexplained changes designed to facilitate another trading abuse.
- **Orleans grains/state:** SecuritiesLoanGrain, AccountGrain, InvestorGrain, RelationshipGrain, PositionGrain, SettlementGrain, SurveillanceGrain; track loan lifecycle, collateral, rates, beneficial owner and intermediaries
- **Required event fields:** loanId, eventTime, lender/borrower/account IDs, beneficialOwnerId, instrumentId, quantity, rate/fee/rebate, collateralType/value, start/end/modify times, finder/intermediary IDs/fees, settlement status
- **Time window(s):** Loan lifecycle; intraday change window; daily collateral/rate checks; 5–30 day repeated counterparty/economic pattern
- **Thresholds/calibration:** Hard checks for reported-vs-actual quantity/rate mismatch or collateral below required policy. Statistical alerts: rate/fee > 99th percentile, finder fee without service evidence, repeated same counterparties, collateral deficit > configured tolerance.
- **Alert evidence:** Loan agreement/lifecycle; reported vs actual fields; collateral calculation; fee flows; related-party graph; linked short/settlement activity; modification history
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
