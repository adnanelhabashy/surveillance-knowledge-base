---
id: CASE-044
type: surveillance-case
case_number: 44
title: "Cross-Market Manipulation"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 44. Cross-Market Manipulation

## Description

One market is manipulated to generate profits in another related market.

## Surveillance families

- [[Families/FAMILY-09|Cross-broker, cross-venue & cross-market manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-042|Cross-Broker Manipulation]]
- [[Cases/CASE-180|Wall-Cross / Market-Sounding Insider Trading]]
- [[Cases/CASE-043|Cross-Venue Manipulation]]
- [[Cases/CASE-540|Cross-Venue Latency Manipulation]]
- [[Cases/CASE-104|Interpositioning Abuse]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Cross-market manipulation** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

## Implementation workspace

- **Rule status:** Initial graph/correlation deterministic model
- **Detection mode:** Rules + relationship/beneficial-owner data; AI/ML optional for unknown clusters
- **Rule logic (starter):** Flag multiple accounts/traders acting with unusually synchronized timing, direction, price/quantity, counterparties or role rotation, especially when relationships, beneficial ownership or common infrastructure indicate common control.
- **Orleans grains/state:** AccountGrain, InvestorGrain, TraderGrain, RelationshipGrain, GroupSurveillanceGrain, InstrumentGrain; maintain temporal co-activity, counterparty graph, beneficial-owner and shared-attribute edges
- **Required event fields:** eventTime, account/trader/investor IDs, beneficialOwnerId, brokerId, instrumentId, side, price, quantity, order/trade IDs, counterparty, device/IP/address/contact/nominee links if legally available
- **Time window(s):** 1–10 s synchronized window; rolling 30 min episode; 1 trading day pattern; 20–60 day relationship baseline
- **Thresholds/calibration:** Start with ≥ 3 accounts or repeated pair, time synchronization ≤ 5 s, same-direction/role correlation ≥ 80%, price within 1 tick, quantity similarity ≥ 80%, and/or strong relationship edge. Escalate when group controls ≥ 20% of instrument activity.
- **Alert evidence:** Graph of accounts/owners/brokers; synchronized event timeline; price/quantity similarity; repeated role rotation; common attributes; group share of volume/depth; resulting price/volume impact
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
