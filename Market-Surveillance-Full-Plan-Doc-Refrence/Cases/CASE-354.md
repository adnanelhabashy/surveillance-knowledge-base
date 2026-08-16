---
id: CASE-354
type: surveillance-case
case_number: 354
title: "Reverse Round-Trip Trading"
status: implementation-seeded
implementation_archetype: wash_matched
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 354. Reverse Round-Trip Trading

## Description

Executing a sequence that transfers a position away and then rapidly returns it to the original economic owner at engineered prices.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-334|Sequence Painting]]
- [[Cases/CASE-353|Synthetic Round-Trip Trading]]
- [[Cases/CASE-499|Out-of-Sequence Print Manipulation]]
- [[Cases/CASE-122|Beneficial-Owner Evasion]]
- [[Cases/CASE-250|Wrap-Fee Trading-Away Cost Concealment]]

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
