---
id: CASE-540
type: surveillance-case
case_number: 540
title: "Cross-Venue Latency Manipulation"
status: implementation-seeded
implementation_archetype: probing_algo
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 540. Cross-Venue Latency Manipulation

## Description

Coordinating orders across venues with intentional false signals so latency differences create profitable executions elsewhere.

## Surveillance families

- [[Families/FAMILY-09|Cross-broker, cross-venue & cross-market manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-480|Selective Latency Advantage Abuse]]
- [[Cases/CASE-043|Cross-Venue Manipulation]]
- [[Cases/CASE-507|Multi-Venue Spoofing]]
- [[Cases/CASE-352|Cross-Venue Circular Trading]]
- [[Cases/CASE-344|Multi-Venue Wash Trading]]

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
