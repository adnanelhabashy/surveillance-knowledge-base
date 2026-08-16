---
id: CASE-449
type: surveillance-case
case_number: 449
title: "Short-Squeeze Ignition"
status: implementation-seeded
implementation_archetype: short_settlement
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 449. Short-Squeeze Ignition

## Description

Using manipulative buying, liquidity withdrawal, or messaging to deliberately trigger forced covering by short sellers.

## Surveillance families

- [[Families/FAMILY-19|Position-limit, corner, squeeze & delivery manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-19|Position Concentration]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-193|Engineered Short Squeeze]]
- [[Cases/CASE-089|Market Squeeze]]
- [[Cases/CASE-442|Stock-Borrow Squeeze Manipulation]]
- [[Cases/CASE-404|Gamma-Squeeze Manipulation Scheme]]
- [[Cases/CASE-532|Algorithmic Momentum Ignition]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic/contextual model
- **Detection mode:** Rules + borrow/locate/settlement data; AI not required
- **Rule logic (starter):** Flag short-sale activity whose marking, locate/borrow status, delivery behavior or reset transactions are inconsistent with rules/data, including repeated fails, sham close-outs or economically continuous short exposure.
- **Orleans grains/state:** AccountGrain, PositionGrain, ShortSaleGrain, SecuritiesLoanGrain, SettlementGrain, InstrumentGrain, SurveillanceGrain; maintain locate inventory, short marks, net positions and fail-to-deliver aging
- **Required event fields:** orderId/tradeId, eventTime, accountId, traderId, instrumentId, side, shortMark/shortExempt, locateId, borrowQty/availability, position, settlementDate, deliveredQty, failQty, closeout/reset trade links, option legs if used
- **Time window(s):** Order-time validation; T+ settlement cycle; rolling 5–20 business-day fail/reset history; circuit-breaker/restricted period windows
- **Thresholds/calibration:** Hard checks for missing/invalid locate or inconsistent marking. Escalate repeated fail amount > 0.5–1% of float/ADV or participant-specific 99th percentile, repeated reset patterns ≥ 3 cycles, and exposure that remains economically short after purported closeout.
- **Alert evidence:** Short-sale mark and locate; borrow availability; net position; settlement/fail history; linked reset/option transactions; restricted/circuit-breaker context; participant repetition
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
