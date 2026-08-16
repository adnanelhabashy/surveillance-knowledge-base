---
id: CASE-508
type: surveillance-case
case_number: 508
title: "Beneficial-Owner Cross-Broker Self-Trade"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 508. Beneficial-Owner Cross-Broker Self-Trade

## Description

Using accounts at different brokers to trade against oneself while concealing common beneficial ownership.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]
- [[Families/FAMILY-09|Cross-broker, cross-venue & cross-market manipulation]]
- [[Families/FAMILY-18|Threshold, beneficial-owner & surveillance-evasion behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]
- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-347|Omnibus Wash Trading]]
- [[Cases/CASE-042|Cross-Broker Manipulation]]
- [[Cases/CASE-535|Self-Trade-Prevention Gaming]]
- [[Cases/CASE-091|Beneficial-Ownership Concealment]]
- [[Cases/CASE-007|Self-Trading]]

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
