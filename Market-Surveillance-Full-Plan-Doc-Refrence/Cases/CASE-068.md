---
id: CASE-068
type: surveillance-case
case_number: 68
title: "Front Running Index Changes"
status: implementation-seeded
implementation_archetype: front_run
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 68. Front Running Index Changes

## Description

Confidential knowledge of upcoming index additions or removals is exploited before public announcement.

## Surveillance families

- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]
- [[Families/FAMILY-12|Front running, trading ahead & misuse of customer-order information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-084|Index-Level Manipulation]]
- [[Cases/CASE-069|Front Running Corporate Actions]]
- [[Cases/CASE-414|RFQ Front Running]]
- [[Cases/CASE-382|Index-Calculation Constituent Marking]]
- [[Cases/CASE-133|Event-Driven Manipulation]]

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
