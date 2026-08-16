---
id: CASE-170
type: surveillance-case
case_number: 170
title: "Auction Indicative-Price Spoofing"
status: implementation-seeded
implementation_archetype: spoof_layer
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 170. Auction Indicative-Price Spoofing

## Description

Large orders are entered during an opening or closing auction to move the theoretical auction price and then cancelled before execution.

## Surveillance families

- [[Families/FAMILY-01|Spoofing, layering & deceptive liquidity]]
- [[Families/FAMILY-05|Opening, closing & auction manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-01|Cancellation Ratio]]
- [[Detectors/DETECTOR-02|Order Lifetime]]
- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-05|Opposite-Side Execution]]

## Related cases

- [[Cases/CASE-011|Closing-Auction Manipulation]]
- [[Cases/CASE-370|Closing-Auction Order Flooding]]
- [[Cases/CASE-012|Opening-Auction Manipulation]]
- [[Cases/CASE-371|Opening-Auction Order Flooding]]
- [[Cases/CASE-401|Continuous-to-Auction Manipulation]]

## SMARTS mapping

- **Public mapping:** This is a narrower variant of a behavior Nasdaq publicly describes SMARTS as monitoring.
- Nasdaq does not publish the full proprietary alert library, so this note does not claim a one-to-one SMARTS alert.

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
