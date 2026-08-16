---
id: CASE-445
type: surveillance-case
case_number: 445
title: "Matched Stock-Loan Transactions"
status: implementation-seeded
implementation_archetype: securities_lending
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 445. Matched Stock-Loan Transactions

## Description

Prearranging securities loans between related parties to transfer value or disguise ownership and availability.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]
- [[Families/FAMILY-14|Securities-lending & settlement manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]
- [[Detectors/DETECTOR-15|Short / Borrow / Settlement Status]]
- [[Detectors/DETECTOR-19|Position Concentration]]

## Related cases

- [[Cases/CASE-235|Stock-Loan Sham Finder-Fee Scheme]]
- [[Cases/CASE-146|Restricted-Stock Loan Default Scheme]]
- [[Cases/CASE-350|Intermediated Matched Trading]]
- [[Cases/CASE-444|Securities-Lending Wash Transaction]]
- [[Cases/CASE-035|Crossing Manipulation]]

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
