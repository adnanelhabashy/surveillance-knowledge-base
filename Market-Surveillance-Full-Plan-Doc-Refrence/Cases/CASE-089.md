---
id: CASE-089
type: surveillance-case
case_number: 89
title: "Market Squeeze"
status: implementation-seeded
implementation_archetype: position_flow
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 89. Market Squeeze

## Description

Control over scarce available securities is exploited to force short sellers or other participants to transact at artificial prices.

## Surveillance families

- [[Families/FAMILY-19|Position-limit, corner, squeeze & delivery manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-19|Position Concentration]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-193|Engineered Short Squeeze]]
- [[Cases/CASE-524|Delivery-Squeeze Manipulation]]
- [[Cases/CASE-088|Cornering the Market]]
- [[Cases/CASE-442|Stock-Borrow Squeeze Manipulation]]
- [[Cases/CASE-449|Short-Squeeze Ignition]]

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
