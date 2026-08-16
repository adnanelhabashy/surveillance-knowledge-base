---
id: CASE-198
type: surveillance-case
case_number: 198
title: "Nested Omnibus Concealment"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 198. Nested Omnibus Concealment

## Description

One financial intermediary trades through another intermediary’s omnibus account so the real underlying investors and coordinated activity become difficult to identify.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]
- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]
- [[Families/FAMILY-18|Threshold, beneficial-owner & surveillance-evasion behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-156|Omnibus-Account Liquidation Abuse]]
- [[Cases/CASE-347|Omnibus Wash Trading]]
- [[Cases/CASE-465|Nominee-Funnel-to-Omnibus Scheme]]
- [[Cases/CASE-037|Related-Account Manipulation]]
- [[Cases/CASE-038|Nominee-Account Manipulation]]

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
