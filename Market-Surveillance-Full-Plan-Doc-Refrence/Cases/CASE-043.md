---
id: CASE-043
type: surveillance-case
case_number: 43
title: "Cross-Venue Manipulation"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 43. Cross-Venue Manipulation

## Description

Activity on one trading venue is used to manipulate the price or behavior on another venue.

## Surveillance families

- [[Families/FAMILY-09|Cross-broker, cross-venue & cross-market manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-352|Cross-Venue Circular Trading]]
- [[Cases/CASE-507|Multi-Venue Spoofing]]
- [[Cases/CASE-540|Cross-Venue Latency Manipulation]]
- [[Cases/CASE-491|False Venue Reporting]]
- [[Cases/CASE-344|Multi-Venue Wash Trading]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

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
