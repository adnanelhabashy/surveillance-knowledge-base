---
id: CASE-018
type: surveillance-case
case_number: 18
title: "Odd-Lot Manipulation"
status: implementation-seeded
implementation_archetype: book_pressure
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 18. Odd-Lot Manipulation

## Description

Using unusually small trades or orders to influence quotations, reference prices, or other market behavior.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]

## Related cases

- [[Cases/CASE-313|Odd-Lot Quote Shaping]]
- [[Cases/CASE-019|Reference-Price Gaming]]
- [[Cases/CASE-500|Off-Exchange Print Price Manipulation]]
- [[Cases/CASE-314|Odd-Lot Last-Sale Marking]]
- [[Cases/CASE-328|Midpoint-Peg Manipulation]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag persistent or rapidly changing one-sided displayed interest that materially distorts depth, spread, best quotes or queue conditions beyond normal liquidity behavior and benefits the initiating participant or related accounts.
- **Orleans grains/state:** OrderBookGrain, TraderGrain, InstrumentGrain, SurveillanceGrain; maintain depth-by-level, spread, imbalance, queue and participant contribution
- **Required event fields:** eventTime, orderId, trader/account IDs, instrumentId, side, price, quantity/displayedQty, action, bestBid/bestAsk, depthByLevel, queuePosition if available, executionQty
- **Time window(s):** 100 ms–5 s book-response window; rolling 30–60 s episode; 5–15 min baseline
- **Thresholds/calibration:** Start with participant displayed depth ≥ 30% of top-5 levels or ≥ 5× median order size, imbalance shift ≥ 30 percentage points, spread change ≥ 2 ticks, persistence/repetition ≥ 3 episodes. Adjust by liquidity/session.
- **Alert evidence:** Before/after depth snapshots; participant share of depth; spread/imbalance changes; order lifecycle; executions/benefit; repetition and related-account activity
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
