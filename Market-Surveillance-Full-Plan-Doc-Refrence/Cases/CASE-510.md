---
id: CASE-510
type: surveillance-case
case_number: 510
title: "Trader-Identifier Switching"
status: implementation-seeded
implementation_archetype: evasion
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 510. Trader-Identifier Switching

## Description

Changing or rotating trader identifiers to conceal continuity of a manipulative strategy.

## Surveillance families

- [[Families/FAMILY-17|Trade-reporting, transaction-publication & identifier manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-511|Algorithm-Identifier Switching]]
- [[Cases/CASE-117|Order-Identity Concealment]]
- [[Cases/CASE-441|Chronic Fail Manipulation]]
- [[Cases/CASE-039|Trader–Investor Collusion]]
- [[Cases/CASE-312|FOK Liquidity-Probing Abuse]]

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
