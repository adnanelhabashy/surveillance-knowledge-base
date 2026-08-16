---
id: CASE-422
type: surveillance-case
case_number: 422
title: "Trading with Knowledge of Order Imbalance"
status: implementation-seeded
implementation_archetype: front_run
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 422. Trading with Knowledge of Order Imbalance

## Description

Using confidential information about a significant order imbalance to trade for personal or favored-account benefit.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-12|Front running, trading ahead & misuse of customer-order information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]

## Related cases

- [[Cases/CASE-421|Trading with Knowledge of Auction Imbalance]]
- [[Cases/CASE-070|Misuse of Client Order Information]]
- [[Cases/CASE-231|Piggybacking / Shadowing Customer Trades]]
- [[Cases/CASE-426|Misuse of Dark-Pool Order Information]]
- [[Cases/CASE-477|Conditional-Order Information Abuse]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Trading with knowledge** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

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
