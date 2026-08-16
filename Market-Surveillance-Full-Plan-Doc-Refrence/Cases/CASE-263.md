---
id: CASE-263
type: surveillance-case
case_number: 263
title: "Intentional Schedule 13D Ownership Concealment"
status: implementation-seeded
implementation_archetype: coordination
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 263. Intentional Schedule 13D Ownership Concealment

## Description

A person deliberately fails to disclose a reportable beneficial-ownership position so the market does not know who controls or is accumulating the stock.

## Surveillance families

- [[Families/FAMILY-05|Opening, closing & auction manipulation]]
- [[Families/FAMILY-18|Threshold, beneficial-owner & surveillance-evasion behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-19|Position Concentration]]

## Related cases

- [[Cases/CASE-091|Beneficial-Ownership Concealment]]
- [[Cases/CASE-265|Late Beneficial-Ownership Filing After Threshold Crossing]]
- [[Cases/CASE-262|Derivative-Based Beneficial-Ownership Evasion]]
- [[Cases/CASE-226|Riskless-Principal Compensation Concealment]]
- [[Cases/CASE-508|Beneficial-Owner Cross-Broker Self-Trade]]

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
