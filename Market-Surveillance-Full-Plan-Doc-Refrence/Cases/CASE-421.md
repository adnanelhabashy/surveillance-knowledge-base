---
id: CASE-421
type: surveillance-case
case_number: 421
title: "Trading with Knowledge of Auction Imbalance"
status: implementation-seeded
implementation_archetype: auction_close
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 421. Trading with Knowledge of Auction Imbalance

## Description

Using non-public or improperly obtained auction imbalance information to trade before the market incorporates it.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-05|Opening, closing & auction manipulation]]
- [[Families/FAMILY-12|Front running, trading ahead & misuse of customer-order information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]

## Related cases

- [[Cases/CASE-422|Trading with Knowledge of Order Imbalance]]
- [[Cases/CASE-372|Late Auction Cancellation Abuse]]
- [[Cases/CASE-400|Auction-to-Continuous Manipulation]]
- [[Cases/CASE-370|Closing-Auction Order Flooding]]
- [[Cases/CASE-371|Opening-Auction Order Flooding]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Trading with knowledge** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag participant activity concentrated in opening/closing or auction windows when it has disproportionate impact on indicative/final price or imbalance, especially if large orders are cancelled late, positions benefit from the result, or the move reverses after the reference point is set.
- **Orleans grains/state:** AuctionGrain, OrderBookGrain, TraderGrain, AccountGrain, PositionGrain, InstrumentGrain, SurveillanceGrain; store indicative price/volume/imbalance series and participant auction contributions
- **Required event fields:** eventTime, sessionPhase, auctionId, orderId, traderId, accountId, instrumentId, side, price, quantity, action, indicativePrice, indicativeVolume, imbalanceQty/side, finalAuctionPrice, referencePrice, executions, position/exposure if available
- **Time window(s):** Entire auction; last 60 s and last 10 s before uncross; final 5–10 min for close-marking; first 5–10 min for open-marking; next-session reversion check
- **Thresholds/calibration:** Start with participant contribution ≥ 20% of auction imbalance/volume, indicative/final impact ≥ 3 ticks or 0.5%, late cancellation ≥ 25% of displayed auction quantity, and/or reversal ≥ 50% of induced move. Use instrument-specific percentiles.
- **Alert evidence:** Indicative-price timeline; imbalance timeline; participant orders/cancels; final uncross executions; price impact attribution; position benefit; post-auction/next-session reversion
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
