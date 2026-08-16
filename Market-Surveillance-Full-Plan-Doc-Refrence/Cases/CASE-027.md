---
id: CASE-027
type: surveillance-case
case_number: 27
title: "Flickering Orders"
status: implementation-seeded
implementation_archetype: message_abuse
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 27. Flickering Orders

## Description

Rapidly placing, modifying, and cancelling orders in ways intended to create misleading order-book signals.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-20|Algorithmic/HFT manipulation, probing & message abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-21|Order-Message Burst Rate]]

## Related cases

- [[Cases/CASE-538|Order-Type Exploitation Manipulation]]
- [[Cases/CASE-315|Micro-Order Spoofing]]
- [[Cases/CASE-025|Order Stuffing]]
- [[Cases/CASE-135|Phishing / Order-Book Phishing]]
- [[Cases/CASE-323|One-Sided Book Pressure Manipulation]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag bursts of order/quote messages whose rate, cancellation intensity and short lifetimes are extreme relative to the instrument/participant baseline and which materially disturb visible liquidity, spreads, queues or matching-engine conditions.
- **Orleans grains/state:** OrderBookGrain, TraderGrain, InstrumentGrain, VenueGrain, SurveillanceGrain; maintain message-rate counters, cancel/replace bursts, queue/depth deltas and throttling/control events
- **Required event fields:** eventTime, orderId, traderId, accountId, instrumentId, venueId, side, price, quantity, action, sequenceNo, messageType, bestBid/bestAsk, depth, reject/throttle/status codes if available
- **Time window(s):** 100 ms, 1 s, 5 s burst windows plus rolling 60 s baseline
- **Thresholds/calibration:** Start with message rate > 99.5th percentile or ≥ 10× participant baseline, cancellation/replace ratio ≥ 90%, median lifetime ≤ 1 s, and measurable spread/depth/queue disruption. Calibrate by venue protocol and liquidity bucket.
- **Alert evidence:** Ordered message sequence; messages/sec; cancel/replace ratio; order lifetimes; queue/depth changes; spread change; venue rejects/throttles; executions gained during/after burst
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
