---
id: CASE-328
type: surveillance-case
case_number: 328
title: "Midpoint-Peg Manipulation"
status: implementation-seeded
implementation_archetype: book_pressure
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 328. Midpoint-Peg Manipulation

## Description

Using orders or trades to move the reference quote so midpoint-pegged orders execute at an artificially favorable level.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-329|Pegged-Order Reference Manipulation]]
- [[Cases/CASE-018|Odd-Lot Manipulation]]
- [[Cases/CASE-487|Midpoint Reference Manipulation by Venue User]]
- [[Cases/CASE-326|Reserve-Order Replenishment Manipulation]]
- [[Cases/CASE-473|Coordinated Quote Narrowing]]

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
