---
id: CASE-511
type: surveillance-case
case_number: 511
title: "Algorithm-Identifier Switching"
status: implementation-seeded
implementation_archetype: probing_algo
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 511. Algorithm-Identifier Switching

## Description

Rotating algorithm or strategy identifiers to evade behavior-based surveillance.

## Surveillance families

- [[Families/FAMILY-17|Trade-reporting, transaction-publication & identifier manipulation]]
- [[Families/FAMILY-20|Algorithmic/HFT manipulation, probing & message abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-21|Order-Message Burst Rate]]
- [[Detectors/DETECTOR-02|Order Lifetime]]
- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]

## Related cases

- [[Cases/CASE-510|Trader-Identifier Switching]]
- [[Cases/CASE-531|Algorithm Gaming]]
- [[Cases/CASE-539|Hidden-Liquidity Algorithm Probing]]
- [[Cases/CASE-117|Order-Identity Concealment]]
- [[Cases/CASE-348|Split-Quantity Matched Trading]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI/ML optional for adaptive algorithm patterns
- **Rule logic (starter):** Flag repeated small/conditional/rapid orders that appear designed to discover hidden liquidity, infer another algorithm/parent order, game queue priority or exploit latency, followed by trading that benefits from the discovered information.
- **Orleans grains/state:** OrderBookGrain, TraderGrain, AlgorithmGrain, InstrumentGrain, VenueGrain, SurveillanceGrain; maintain probe-response sequences, queue position, hidden-liquidity reveals and algorithm identifiers
- **Required event fields:** eventTime with high precision, orderId, trader/account/algorithmId, instrumentId, venueId, orderType/IOC/FOK/minQty/peg, side, price, qty, cancel/replace, fillQty, queuePosition if available, hidden/iceberg indication, market response
- **Time window(s):** Microsecond/ms–1 s probe-response; rolling 5–60 s sequence; 5–30 min repeated-pattern baseline
- **Thresholds/calibration:** Start with ≥ 5 probe attempts in 10 s, small size ≤ 10–20% median trade/order size, response/fill pattern repeatedly followed by larger trade within ≤ 1 s, success rate materially above baseline, or queue-priority gain after cancel/reenter. Venue-specific calibration is essential.
- **Alert evidence:** High-resolution ordered event trace; probe sizes/types; hidden-liquidity/queue response; subsequent larger executions; queue-position advantage; algorithm/account identity changes; repeated success statistics
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
