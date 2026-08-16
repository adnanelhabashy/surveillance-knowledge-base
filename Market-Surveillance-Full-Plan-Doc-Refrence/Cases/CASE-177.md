---
id: CASE-177
type: surveillance-case
case_number: 177
title: "Rights-Issue Reference-Price Manipulation"
status: implementation-seeded
implementation_archetype: benchmark
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 177. Rights-Issue Reference-Price Manipulation

## Description

Shares or rights are manipulated around theoretical prices so that an artificial arbitrage opportunity is created during a rights issue.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]
- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]
- [[Families/FAMILY-24|Tender, corporate-action & record-date trading manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]

## Related cases

- [[Cases/CASE-176|Artificial Arbitrage Creation]]
- [[Cases/CASE-394|Rights-to-Underlying Manipulation]]
- [[Cases/CASE-399|Dark-Pool Reference-Price Manipulation]]
- [[Cases/CASE-133|Event-Driven Manipulation]]
- [[Cases/CASE-500|Off-Exchange Print Price Manipulation]]

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
