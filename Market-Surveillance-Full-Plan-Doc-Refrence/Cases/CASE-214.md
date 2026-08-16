---
id: CASE-214
type: surveillance-case
case_number: 214
title: "Pressure-to-Alter-Quote Scheme"
status: implementation-seeded
implementation_archetype: book_pressure
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 214. Pressure-to-Alter-Quote Scheme

## Description

One market participant pressures or directs another market maker to raise, lower, or maintain its quotation for an improper purpose.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-15|Market-maker, liquidity-provider & quote-coordination abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-215|Market-Maker Retaliation / Intimidation]]
- [[Cases/CASE-474|Market-Maker Quote Synchronization]]
- [[Cases/CASE-210|Closing-Bid Marking]]
- [[Cases/CASE-471|Liquidity-Program Gaming]]
- [[Cases/CASE-494|False Account-Type Reporting]]

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
