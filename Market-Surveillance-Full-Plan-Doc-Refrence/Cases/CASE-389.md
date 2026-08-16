---
id: CASE-389
type: surveillance-case
case_number: 389
title: "Dual-Listed Share Manipulation"
status: implementation-seeded
implementation_archetype: cross_product
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 389. Dual-Listed Share Manipulation

## Description

Using activity in one listing of a security to manipulate the price or execution in another listing.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-398|Dark-to-Lit Manipulation]]
- [[Cases/CASE-036|Collusive Trading]]
- [[Cases/CASE-015|Price Depressing]]
- [[Cases/CASE-014|Price Ramping / Price Run-Up]]
- [[Cases/CASE-500|Off-Exchange Print Price Manipulation]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag coordinated activity across economically related instruments when orders/trades in one product materially influence another and the participant holds a position that benefits from the induced move, especially near expiry, strike, barrier or index/ETF calculation events.
- **Orleans grains/state:** InstrumentGrain, OrderBookGrain per product, PositionGrain, TraderGrain, RelationshipGrain, BenchmarkGrain where applicable, SurveillanceGrain; maintain product-link graph and synchronized event windows
- **Required event fields:** eventTime, traderId, accountId, beneficialOwnerId, instrumentId, relatedInstrumentId, productType, underlyingId, side, price, quantity, order/trade action, delta/exposure if available, expiry/strike/barrier, indexWeight/ETF relationship, venueId
- **Time window(s):** 100 ms–60 s synchronized trading window; 5–30 min manipulation episode; expiry/settlement event window; daily exposure context
- **Thresholds/calibration:** Start with cross-product correlation/sequence within ≤ 5 s, manipulated-leg participation ≥ 10–20%, price impact ≥ 2–3 ticks, and positive economic benefit in related position. For pinning/barriers require proximity within 0.25–0.5% of trigger/reference.
- **Alert evidence:** Linked instruments; synchronized timelines; order-book impact on manipulated leg; related position/exposure; estimated economic benefit; expiry/strike/barrier/reference context
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
