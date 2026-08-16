---
id: CASE-365
type: surveillance-case
case_number: 365
title: "Quarter-End Marking"
status: implementation-seeded
implementation_archetype: price_momentum
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 365. Quarter-End Marking

## Description

Trading near quarter-end to influence valuations, performance, collateral, or reported positions.

## Surveillance families

- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-366|Year-End Marking]]
- [[Cases/CASE-364|End-of-Month Marking]]
- [[Cases/CASE-083|Settlement-Price Manipulation]]
- [[Cases/CASE-221|Investment-Fund Asset Overvaluation]]
- [[Cases/CASE-222|Performance-Fee Inflation Through False Valuation]]

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
