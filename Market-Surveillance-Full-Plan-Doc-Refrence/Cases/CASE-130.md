---
id: CASE-130
type: surveillance-case
case_number: 130
title: "Closing-Price + Derivative Manipulation"
status: implementation-seeded
implementation_archetype: auction_close
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 130. Closing-Price + Derivative Manipulation

## Description

The stock’s closing price is manipulated because that price determines the value or settlement of a related derivative.

## Surveillance families

- [[Families/FAMILY-05|Opening, closing & auction manipulation]]
- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]

## Related cases

- [[Cases/CASE-454|Closing-Price Pump]]
- [[Cases/CASE-046|Underlying-vs-Derivative Manipulation]]
- [[Cases/CASE-376|Closing-Cross Participation Manipulation]]
- [[Cases/CASE-355|Closing-Print Wash Trading]]
- [[Cases/CASE-009|Marking the Close]]

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
