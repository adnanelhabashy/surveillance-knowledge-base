---
id: CASE-478
type: surveillance-case
case_number: 478
title: "IOI Leakage Trading Abuse"
status: implementation-seeded
implementation_archetype: front_run
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 478. IOI Leakage Trading Abuse

## Description

Leaking or exploiting confidential indications of interest to enable trading ahead or adverse price changes.

## Surveillance families

- [[Families/FAMILY-12|Front running, trading ahead & misuse of customer-order information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-415|RFQ Information Leakage Abuse]]
- [[Cases/CASE-272|Dark-Pool Subscriber Information Trading Abuse]]
- [[Cases/CASE-417|Front Running Block Crosses]]
- [[Cases/CASE-419|Front Running Tender Activity]]
- [[Cases/CASE-418|Front Running Offerings]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

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
