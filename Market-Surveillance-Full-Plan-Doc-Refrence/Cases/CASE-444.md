---
id: CASE-444
type: surveillance-case
case_number: 444
title: "Securities-Lending Wash Transaction"
status: implementation-seeded
implementation_archetype: securities_lending
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 444. Securities-Lending Wash Transaction

## Description

Arranging offsetting stock-loan transactions with no genuine economic purpose to manufacture activity, fees, or regulatory appearances.

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
- [[Cases/CASE-190|Liquidity-Rebate Wash Trading]]
- [[Cases/CASE-236|Stock-Loan Kickback Scheme]]
- [[Cases/CASE-445|Matched Stock-Loan Transactions]]
- [[Cases/CASE-443|Borrow-Fee Manipulation]]

## SMARTS mapping

- **Public mapping:** This is a narrower variant of a behavior Nasdaq publicly describes SMARTS as monitoring.
- Nasdaq does not publish the full proprietary alert library, so this note does not claim a one-to-one SMARTS alert.

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
