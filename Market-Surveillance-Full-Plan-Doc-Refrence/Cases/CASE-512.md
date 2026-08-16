---
id: CASE-512
type: surveillance-case
case_number: 512
title: "Subaccount Cycling"
status: implementation-seeded
implementation_archetype: evasion
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 512. Subaccount Cycling

## Description

Moving manipulative activity among subaccounts under a master account to stay below alert thresholds.

## Surveillance families

- [[Families/FAMILY-18|Threshold, beneficial-owner & surveillance-evasion behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-19|Position Concentration]]

## Related cases

- [[Cases/CASE-157|Master/Subaccount Anonymity Abuse]]
- [[Cases/CASE-514|Cross-Security Threshold Evasion]]
- [[Cases/CASE-118|Order Splitting to Evade Surveillance]]
- [[Cases/CASE-120|Broker-Hopping / Venue-Hopping Evasion]]
- [[Cases/CASE-119|Multi-Account Threshold Evasion]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial aggregation/evasion model
- **Detection mode:** Rules + identity/relationship resolution; AI not required
- **Rule logic (starter):** Flag behavior intentionally fragmented or relabeled across orders, accounts, brokers, securities, identifiers or time windows such that the combined economic activity breaches a surveillance/risk condition even though each fragment stays below it.
- **Orleans grains/state:** TraderGrain, AccountGrain, InvestorGrain, RelationshipGrain, InstrumentGrain, GroupSurveillanceGrain; maintain canonical identity, rolling aggregate activity and identifier-change history
- **Required event fields:** eventTime, order/trade IDs, trader/account/investor/beneficialOwner IDs, brokerId, algorithmId, subaccountId, instrumentId/relatedInstrumentId, side, price, quantity, action, identifier-change events
- **Time window(s):** 1–60 s micro-fragmentation; 30 min episode; trading-day aggregate; 20–60 day identity/relationship history
- **Thresholds/calibration:** Recombine linked fragments first, then apply the underlying detector threshold. Starter escalation: ≥ 3 fragments/identities, each near 50–100% of a configured threshold, combined activity ≥ 120% of threshold, or repeated identifier rotation ≥ 3 times/day.
- **Alert evidence:** Canonical entity graph; fragment timeline; individual vs combined detector values; identifier/subaccount/broker changes; linked instruments; resulting market impact and repeated evasion history
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
