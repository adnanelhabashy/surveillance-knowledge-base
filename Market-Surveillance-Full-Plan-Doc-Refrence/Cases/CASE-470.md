---
id: CASE-470
type: surveillance-case
case_number: 470
title: "Market-Share Incentive Wash Trading"
status: implementation-seeded
implementation_archetype: wash_matched
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 470. Market-Share Incentive Wash Trading

## Description

Using non-economic trading to inflate reported market share and earn volume-based incentives.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]

## Related cases

- [[Cases/CASE-190|Liquidity-Rebate Wash Trading]]
- [[Cases/CASE-355|Closing-Print Wash Trading]]
- [[Cases/CASE-356|Opening-Print Wash Trading]]
- [[Cases/CASE-347|Omnibus Wash Trading]]
- [[Cases/CASE-379|Settlement-Window Wash Trading]]

## SMARTS mapping

- **Public mapping:** This is a narrower variant of a behavior Nasdaq publicly describes SMARTS as monitoring.
- Nasdaq does not publish the full proprietary alert library, so this note does not claim a one-to-one SMARTS alert.

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
