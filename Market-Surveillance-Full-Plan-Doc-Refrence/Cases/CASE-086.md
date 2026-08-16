---
id: CASE-086
type: surveillance-case
case_number: 86
title: "Benchmark Manipulation"
status: implementation-seeded
implementation_archetype: benchmark
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 86. Benchmark Manipulation

## Description

Transactions or submissions are intentionally designed to distort a market benchmark.

## Surveillance families

- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-081|VWAP Manipulation]]
- [[Cases/CASE-357|Benchmark-Window Wash Trading]]
- [[Cases/CASE-381|Benchmark-Window Concentration]]
- [[Cases/CASE-420|Front Running Benchmark Fixes]]
- [[Cases/CASE-082|TWAP Manipulation]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Benchmark manipulation** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

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
