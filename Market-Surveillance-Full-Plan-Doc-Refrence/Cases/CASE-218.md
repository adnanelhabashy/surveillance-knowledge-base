---
id: CASE-218
type: surveillance-case
case_number: 218
title: "Non-Firm Stated-Price Offer"
status: implementation-seeded
implementation_archetype: book_pressure
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 218. Non-Firm Stated-Price Offer

## Description

A broker advertises an offer to buy or sell at a stated price even though it is not genuinely prepared to transact under those conditions.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-295|Best-Offer Spoofing]]
- [[Cases/CASE-160|Alternative Merger / Exchange-Offer Price Manipulation]]
- [[Cases/CASE-032|Dominant Offer Manipulation]]
- [[Cases/CASE-256|Tender-Offer Outside-Purchase / Unequal-Consideration Abuse]]
- [[Cases/CASE-208|Fictitious Quotation]]

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
