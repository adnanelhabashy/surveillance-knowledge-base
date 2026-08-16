---
id: CASE-383
type: surveillance-case
case_number: 383
title: "NAV-Strike Price Manipulation"
status: implementation-seeded
implementation_archetype: benchmark
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 383. NAV-Strike Price Manipulation

## Description

Manipulating underlying prices near a fund or derivative NAV strike time to affect valuation or settlement.

## Surveillance families

- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]
- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]

## Related cases

- [[Cases/CASE-173|Strike / Barrier Pinning]]
- [[Cases/CASE-223|Fraudulent NAV Pricing]]
- [[Cases/CASE-083|Settlement-Price Manipulation]]
- [[Cases/CASE-017|Mini-Manipulation]]
- [[Cases/CASE-387|Component-to-ETF Manipulation]]

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
