---
id: CASE-229
type: surveillance-case
case_number: 229
title: "Cancel-and-Rebill Manipulation"
status: implementation-seeded
implementation_archetype: broker_conduct
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 229. Cancel-and-Rebill Manipulation

## Description

An executed trade is improperly cancelled and rebooked with altered account, price, capacity, or other information to shift gains, losses, or regulatory consequences.

## Surveillance families

- [[Families/FAMILY-25|Client/proprietary cross-trade and execution-value-transfer abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-227|Error-Account Abuse]]
- [[Cases/CASE-225|Personal-Account Cross-Trade Abuse]]
- [[Cases/CASE-195|Account-Takeover Options Cross-Trade Fraud]]
- [[Cases/CASE-492|False Capacity Reporting]]
- [[Cases/CASE-504|Average-Price Allocation Concealment]]

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
