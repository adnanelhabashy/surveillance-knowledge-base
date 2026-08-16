---
id: CASE-171
type: surveillance-case
case_number: 171
title: "Auction Uncrossing-Volume Manipulation"
status: implementation-seeded
implementation_archetype: auction_close
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 171. Auction Uncrossing-Volume Manipulation

## Description

Orders are intentionally used to distort indicative auction volume or imbalance even when the manipulator does not genuinely intend to trade that quantity.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]
- [[Families/FAMILY-05|Opening, closing & auction manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]
- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]

## Related cases

- [[Cases/CASE-370|Closing-Auction Order Flooding]]
- [[Cases/CASE-371|Opening-Auction Order Flooding]]
- [[Cases/CASE-401|Continuous-to-Auction Manipulation]]
- [[Cases/CASE-372|Late Auction Cancellation Abuse]]
- [[Cases/CASE-170|Auction Indicative-Price Spoofing]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

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
