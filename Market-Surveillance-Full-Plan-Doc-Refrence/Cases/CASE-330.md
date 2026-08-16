---
id: CASE-330
type: surveillance-case
case_number: 330
title: "Last-Sale Price Marking"
status: implementation-seeded
implementation_archetype: price_momentum
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 330. Last-Sale Price Marking

## Description

Executing trades primarily to move or maintain the published last-sale price at an artificial level.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-360|Off-Market Cross Price Marking]]
- [[Cases/CASE-314|Odd-Lot Last-Sale Marking]]
- [[Cases/CASE-361|Negotiated-Cross Price Marking]]
- [[Cases/CASE-332|Intraday Low Marking]]
- [[Cases/CASE-331|Intraday High Marking]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; external promotion data may enrich pump/rumor variants
- **Rule logic (starter):** Flag concentrated directional trading that creates an abnormal price/volume move, followed by distribution, reversal or economic benefit to the initiating participant/group, especially in low-liquidity/low-float securities.
- **Orleans grains/state:** OrderBookGrain, TradeGrain, TraderGrain, AccountGrain, PositionGrain, InstrumentGrain, GroupSurveillanceGrain; maintain returns, ADV/volume baselines, participant contribution and inventory changes
- **Required event fields:** eventTime, order/trade IDs, trader/account IDs, instrumentId, side, price, quantity, aggressor flag, bestBid/bestAsk, volume, float/ADV, positionBefore/After, relatedAccountGroup
- **Time window(s):** 1–5 min ignition window; 30–60 min episode; same-day accumulation/distribution; 5–20 day pump/dump context
- **Thresholds/calibration:** Start with return ≥ 3% or > 5× normal short-window volatility, volume ≥ 3× baseline, participant/group ≥ 20% of aggressive volume, and subsequent reversal/distribution ≥ 30% of induced move/position. Use lower thresholds for illiquid names via percentiles.
- **Alert evidence:** Price/volume chart; participant contribution; aggressive trade sequence; inventory accumulation/distribution; group links; market depth; reversion; realized/mark-to-market benefit
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
