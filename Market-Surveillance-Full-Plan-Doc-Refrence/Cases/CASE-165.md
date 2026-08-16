---
id: CASE-165
type: surveillance-case
case_number: 165
title: "Mark-Down → Accumulate → Mark-Up → Distribute Scheme"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 165. Mark-Down → Accumulate → Mark-Up → Distribute Scheme

## Description

Coordinated accounts first push a thinly traded stock downward to accumulate, then push it upward before unloading the position.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]
- [[Families/FAMILY-21|Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-19|Position Concentration]]

## Related cases

- [[Cases/CASE-451|Accumulation-Promotion-Distribution Scheme]]
- [[Cases/CASE-154|Deposit–Sell–Wire-Out Scheme]]
- [[Cases/CASE-207|Direct-To-Investor Stock-Manipulation Scam]]
- [[Cases/CASE-053|Microcap / Penny-Stock Manipulation]]
- [[Cases/CASE-194|Hacked-Account Forced-Buy Pump]]

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
