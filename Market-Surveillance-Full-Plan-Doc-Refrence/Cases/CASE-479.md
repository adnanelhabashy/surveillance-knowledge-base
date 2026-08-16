---
id: CASE-479
type: surveillance-case
case_number: 479
title: "Venue-Operator Proprietary Front Running"
status: implementation-seeded
implementation_archetype: front_run
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 479. Venue-Operator Proprietary Front Running

## Description

A venue operator or affiliate uses knowledge of customer orders to trade proprietarily ahead of them.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]
- [[Families/FAMILY-12|Front running, trading ahead & misuse of customer-order information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-272|Dark-Pool Subscriber Information Trading Abuse]]
- [[Cases/CASE-231|Piggybacking / Shadowing Customer Trades]]
- [[Cases/CASE-414|RFQ Front Running]]
- [[Cases/CASE-247|Affiliated-Venue Routing Conflict]]
- [[Cases/CASE-491|False Venue Reporting]]

## SMARTS mapping

- **Public mapping:** This is a narrower variant of a behavior Nasdaq publicly describes SMARTS as monitoring.
- Nasdaq does not publish the full proprietary alert library, so this note does not claim a one-to-one SMARTS alert.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + customer/order-source data; AI not required
- **Rule logic (starter):** Flag proprietary/personal/favored-account trading shortly before a known customer, parent, block, research or other market-moving order/event, followed by favorable price movement and offsetting/beneficial execution.
- **Orleans grains/state:** TraderGrain, AccountGrain, OrderGrain, ParentOrderGrain, RelationshipGrain, InstrumentGrain, SurveillanceGrain; preserve order-receipt timestamps and trader access to customer flow
- **Required event fields:** customerOrderId/parentOrderId, receiveTime, releaseTime, traderId, employee/account classification, instrumentId, side, price, quantity, proprietary/personal order IDs, execution times/prices, customer execution, access/desk relationship
- **Time window(s):** 100 ms–5 min before customer execution for electronic flow; up to 30–60 min for blocks/research/events; exit/profit window through completion and shortly after
- **Thresholds/calibration:** Start with same/related instrument, same directional anticipation, proprietary trade before customer order by ≤ 5 min, customer order materially moves price ≥ 1–2 ticks, and pre-trade account realizes favorable markout. Use desk-specific baselines.
- **Alert evidence:** Customer order receipt/release/execution timeline; employee/proprietary order timeline; access relationship; price markout; P&L; repeated episodes
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
