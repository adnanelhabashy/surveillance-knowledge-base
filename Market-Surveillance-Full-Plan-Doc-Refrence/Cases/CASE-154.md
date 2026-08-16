---
id: CASE-154
type: surveillance-case
case_number: 154
title: "Deposit–Sell–Wire-Out Scheme"
status: implementation-seeded
implementation_archetype: position_flow
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 154. Deposit–Sell–Wire-Out Scheme

## Description

Large blocks of thinly traded stock are deposited, quickly liquidated, and the cash proceeds immediately transferred out of the brokerage account.

## Surveillance families

- [[Families/FAMILY-21|Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-19|Position Concentration]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-165|Mark-Down → Accumulate → Mark-Up → Distribute Scheme]]
- [[Cases/CASE-194|Hacked-Account Forced-Buy Pump]]
- [[Cases/CASE-207|Direct-To-Investor Stock-Manipulation Scam]]
- [[Cases/CASE-053|Microcap / Penny-Stock Manipulation]]
- [[Cases/CASE-197|Share-Journaling Fragmentation Scheme]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial graph/flow deterministic model
- **Detection mode:** Rules + ownership/transfer/reference data; AI not required
- **Rule logic (starter):** Flag abnormal acquisition, concentration, transfer, journaling, liquidation or value-movement chains that obscure common control or rapidly monetize thinly traded positions.
- **Orleans grains/state:** PositionGrain, AccountGrain, InvestorGrain, RelationshipGrain, TransferGrain, InstrumentGrain, SurveillanceGrain; maintain provenance and ownership graph
- **Required event fields:** eventTime, account/investor/beneficialOwner IDs, instrumentId, transfer/deposit/journal event, source/destination account, quantity, cost basis if available, restriction/legend status, subsequent orders/trades, cash withdrawal/wire
- **Time window(s):** Same day to 5 trading days for deposit-to-liquidation; 20–90 days for concentration/control patterns; full provenance chain where available
- **Thresholds/calibration:** Start with deposited/transferred position > 10–20% ADV or free float, liquidation of ≥ 50% within 1–3 days, fragmentation across ≥ 3 accounts, or common-control concentration > configured disclosure/market-risk threshold.
- **Alert evidence:** Position provenance graph; source/destination accounts; beneficial owners; deposit/journal records; liquidation trades; cash movement; share of ADV/float; related-account links
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
