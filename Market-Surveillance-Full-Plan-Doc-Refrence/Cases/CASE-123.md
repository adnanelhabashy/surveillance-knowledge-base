---
id: CASE-123
type: surveillance-case
case_number: 123
title: "Spoof-and-Trade"
status: implementation-seeded
implementation_archetype: spoof_layer
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 123. Spoof-and-Trade

## Description

Fake orders are placed on one side of the book while genuine trades are executed on the opposite side.

## Surveillance families

- [[Families/FAMILY-01|Spoofing, layering & deceptive liquidity]]

## Reusable detector starting points

- [[Detectors/DETECTOR-01|Cancellation Ratio]]
- [[Detectors/DETECTOR-02|Order Lifetime]]
- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-05|Opposite-Side Execution]]

## Related cases

- [[Cases/CASE-237|Neighboring Options-Series Spoofing]]
- [[Cases/CASE-327|Iceberg-Spoof Combination]]
- [[Cases/CASE-129|Cross-Product Spoofing]]
- [[Cases/CASE-100|Unauthorized Trading]]
- [[Cases/CASE-315|Micro-Order Spoofing]]

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
