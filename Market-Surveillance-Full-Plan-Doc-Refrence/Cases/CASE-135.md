---
id: CASE-135
type: surveillance-case
case_number: 135
title: "Phishing / Order-Book Phishing"
status: implementation-seeded
implementation_archetype: probing_algo
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 135. Phishing / Order-Book Phishing

## Description

Trades or small orders are executed to uncover another participant’s hidden orders, after which the trader exploits the discovered information.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-315|Micro-Order Spoofing]]
- [[Cases/CASE-025|Order Stuffing]]
- [[Cases/CASE-027|Flickering Orders]]
- [[Cases/CASE-323|One-Sided Book Pressure Manipulation]]
- [[Cases/CASE-324|Book-Pressure Flip Manipulation]]

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
