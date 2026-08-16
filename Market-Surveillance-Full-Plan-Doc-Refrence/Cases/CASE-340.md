---
id: CASE-340
type: surveillance-case
case_number: 340
title: "Profit-Transfer Matched Trading"
status: implementation-seeded
implementation_archetype: wash_matched
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 340. Profit-Transfer Matched Trading

## Description

Using coordinated matched trades at selected prices to shift profits from one account to another.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]
- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-341|Loss-Transfer Matched Trading]]
- [[Cases/CASE-350|Intermediated Matched Trading]]
- [[Cases/CASE-126|Pump + Matched Orders]]
- [[Cases/CASE-004|Matched Orders / Matched Trading]]
- [[Cases/CASE-348|Split-Quantity Matched Trading]]

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
