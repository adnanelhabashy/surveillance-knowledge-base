---
id: CASE-001
type: surveillance-case
case_number: 1
title: "Spoofing"
status: implementation-seeded
implementation_archetype: spoof_layer
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 1. Spoofing

## Description

Placing orders without genuine intent to execute them to create false buying or selling pressure.

## Surveillance families

- [[Families/FAMILY-01|Spoofing, layering & deceptive liquidity]]

## Reusable detector starting points

- [[Detectors/DETECTOR-01|Cancellation Ratio]]
- [[Detectors/DETECTOR-02|Order Lifetime]]
- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-05|Opposite-Side Execution]]

## Related cases

- [[Cases/CASE-507|Multi-Venue Spoofing]]
- [[Cases/CASE-315|Micro-Order Spoofing]]
- [[Cases/CASE-296|Away-From-Touch Spoofing]]
- [[Cases/CASE-295|Best-Offer Spoofing]]
- [[Cases/CASE-506|Multi-Broker Spoofing]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Spoofing** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag when a participant displays unusually large non-bona-fide-looking interest, cancels most of it quickly after influencing book/price, and either trades on the opposite side or gains an execution advantage. For layering, also require multiple price levels or repeated waves.
- **Orleans grains/state:** OrderBookGrain, TraderGrain, AccountGrain, InstrumentGrain, SurveillanceGrain; keep live depth, per-order lifecycle, participant cancellation/execution stats and opposite-side fills
- **Required event fields:** eventId, eventTime, orderId, parentOrderId/algorithmId if available, traderId, accountId, beneficialOwnerId if available, instrumentId, venueId, side, orderType, price, quantity, displayedQuantity, action(new/modify/cancel), executionQty, executionPrice, bestBid/bestAsk, depthByLevel
- **Time window(s):** Primary 1–5 s order-lifecycle window; rolling 30–60 s participant/instrument window; 5–15 min repetition window
- **Thresholds/calibration:** Start with displayed size ≥ 5× instrument median, cancellation ratio ≥ 80%, lifetime ≤ 3 s, price/book impact ≥ 1 tick, opposite-side execution within 10 s; layering: ≥ 3 price levels. Replace with liquidity-bucket percentiles during calibration.
- **Alert evidence:** Full order lifecycle; before/after order-book snapshots; cancelled quantity; price/depth impact; opposite-side executions; participant metrics; repeated similar episodes; related-account links if relevant
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
