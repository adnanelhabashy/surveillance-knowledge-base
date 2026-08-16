---
id: CASE-468
type: surveillance-case
case_number: 468
title: "Rebate-Farming Self-Match"
status: implementation-seeded
implementation_archetype: wash_matched
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 468. Rebate-Farming Self-Match

## Description

Generating self-matches or wash-like trades primarily to earn maker rebates or other venue incentives.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]

## Related cases

- [[Cases/CASE-190|Liquidity-Rebate Wash Trading]]
- [[Cases/CASE-535|Self-Trade-Prevention Gaming]]
- [[Cases/CASE-355|Closing-Print Wash Trading]]
- [[Cases/CASE-356|Opening-Print Wash Trading]]
- [[Cases/CASE-344|Multi-Venue Wash Trading]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required; ownership/reference data strongly improves detection
- **Rule logic (starter):** Flag executions that produce no meaningful change in beneficial ownership or show repeated highly synchronized counterparties, prices and quantities consistent with matched/prearranged/circular transfer rather than competitive trading.
- **Orleans grains/state:** TradeGrain, AccountGrain, InvestorGrain, RelationshipGrain, InstrumentGrain, SurveillanceGrain; maintain counterparty pairs, beneficial-owner mapping, trade cycles and rolling transferred quantity
- **Required event fields:** tradeId, eventTime, buyAccountId, sellAccountId, buyTraderId, sellTraderId, buyBeneficialOwnerId, sellBeneficialOwnerId, instrumentId, price, quantity, venueId, orderIds, tradeCondition
- **Time window(s):** Immediate same-trade check; 1–60 s matched-order window; rolling 30 min and trading-day counterparty/cycle windows
- **Thresholds/calibration:** Self/wash: same beneficial owner is a high-severity condition. Matched: start with price equal/within 1 tick, quantity similarity ≥ 90%, time difference ≤ 5 s and ≥ 3 repeated pairings in 30 min. Circular: cycle length 3–6 accounts within 30 min and quantity similarity ≥ 80%.
- **Alert evidence:** Matched buy/sell order IDs; counterparties and beneficial owners; timestamps; price/quantity similarity; repeated pair matrix; circular path/graph; percentage of participant and market volume
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
