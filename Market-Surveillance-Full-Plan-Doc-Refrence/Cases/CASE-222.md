---
id: CASE-222
type: surveillance-case
case_number: 222
title: "Performance-Fee Inflation Through False Valuation"
status: implementation-seeded
implementation_archetype: broker_conduct
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 222. Performance-Fee Inflation Through False Valuation

## Description

Portfolio assets are intentionally marked too high so an adviser can report exaggerated returns or collect larger management/performance fees.

## Surveillance families

- [[Families/FAMILY-06|Benchmark, VWAP, TWAP, NAV & settlement manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-221|Investment-Fund Asset Overvaluation]]
- [[Cases/CASE-364|End-of-Month Marking]]
- [[Cases/CASE-220|Mark-to-Market Inflation]]
- [[Cases/CASE-268|Security-Based-Swap Valuation Manipulation]]
- [[Cases/CASE-365|Quarter-End Marking]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + account, fee, authorization and broker records; AI not required
- **Rule logic (starter):** Flag broker/customer-account conduct where executions, allocations, fees, account usage or crosses systematically transfer value, generate excessive compensation, avoid controls or occur without the required authorization/context.
- **Orleans grains/state:** AccountGrain, BrokerGrain, TraderGrain, TradeGrain, FeeGrain, PositionGrain, RelationshipGrain, SurveillanceGrain; preserve customer mandate, fee schedule, capacity and allocation history
- **Required event fields:** eventTime, account/customer/trader/broker IDs, order/trade IDs, instrumentId, side, price, quantity, capacity, authorization/mandate, commission/markup/markdown/fee, market reference price, allocation/error-account/cancel-rebill fields, related personal/proprietary account
- **Time window(s):** Per trade; daily account activity; rolling 30/90-day turnover, fee and transfer patterns
- **Thresholds/calibration:** Hard alert for missing authorization or prohibited account relationship where configured. Behavioral starter: turnover/fee ratio > 99th percentile, price disadvantage > 2–5× normal spread, repeated value transfer ≥ 3 events, or commissions/fees materially disproportionate to account size/activity.
- **Alert evidence:** Customer mandate/authorization; execution and reference price; fee calculation; account turnover; allocation/cancel-rebill history; related proprietary/personal account; quantified value transfer/customer harm
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
