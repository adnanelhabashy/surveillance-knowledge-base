---
id: CASE-301
type: surveillance-case
case_number: 301
title: "Fake Resistance Wall"
status: implementation-seeded
implementation_archetype: spoof_layer
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 301. Fake Resistance Wall

## Description

Displaying large non-genuine offers intended to create an artificial impression of strong selling resistance.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-300|Fake Support Wall]]
- [[Cases/CASE-299|Moving-Wall Manipulation]]
- [[Cases/CASE-296|Away-From-Touch Spoofing]]
- [[Cases/CASE-295|Best-Offer Spoofing]]
- [[Cases/CASE-315|Micro-Order Spoofing]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

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
