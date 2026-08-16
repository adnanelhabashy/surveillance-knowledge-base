---
id: CASE-268
type: surveillance-case
case_number: 268
title: "Security-Based-Swap Valuation Manipulation"
status: implementation-seeded
implementation_archetype: benchmark
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 268. Security-Based-Swap Valuation Manipulation

## Description

A participant manipulates information, underlying markets, or other inputs specifically to distort the price or valuation of a security-based swap.

## Surveillance families

- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]
- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]

## Related cases

- [[Cases/CASE-267|Security-Based-Swap Payment/Delivery Manipulation]]
- [[Cases/CASE-266|Manufactured Credit Event]]
- [[Cases/CASE-160|Alternative Merger / Exchange-Offer Price Manipulation]]
- [[Cases/CASE-383|NAV-Strike Price Manipulation]]
- [[Cases/CASE-176|Artificial Arbitrage Creation]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag trading concentrated inside a benchmark/reference calculation window when the participant contributes disproportionate volume or directional price impact and has an economic exposure that benefits from moving the benchmark.
- **Orleans grains/state:** BenchmarkGrain, OrderBookGrain, TradeGrain, PositionGrain, TraderGrain, InstrumentGrain, SurveillanceGrain; retain benchmark constituents, calculation windows, participant contribution and before/after prices
- **Required event fields:** eventTime, tradeId/orderId, traderId, accountId, instrumentId, price, quantity, side, benchmarkId, benchmarkWindowStart/End, benchmarkValue/referencePrice, constituentWeight, position/exposure, venueId
- **Time window(s):** Exact benchmark calculation window plus 5–30 min before/after; daily historical baseline; position horizon through settlement if applicable
- **Thresholds/calibration:** Start with participant ≥ 20% of window volume or top 1% historical participation, directional impact ≥ 3 ticks/0.5%, concentration in final 20% of window, and demonstrable exposure benefit. Calibrate separately for VWAP/TWAP/settlement/NAV/reference-price cases.
- **Alert evidence:** Benchmark formula/window; participant trades; contribution to volume/value; counterfactual benchmark excluding participant; exposure/P&L benefit; pre/post-window reversion
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
