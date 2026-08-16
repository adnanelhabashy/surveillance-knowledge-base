---
id: CASE-138
type: surveillance-case
case_number: 138
title: "Trading-Safeguard Bypass Manipulation"
status: implementation-seeded
implementation_archetype: evasion
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 138. Trading-Safeguard Bypass Manipulation

## Description

Orders or transactions are structured specifically to circumvent market controls such as volume limits, price limits, or spread protections.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-033|Bid-Ask Spread Manipulation]]
- [[Cases/CASE-321|Spread-Widening Manipulation]]
- [[Cases/CASE-322|Spread-Narrowing Manipulation]]
- [[Cases/CASE-293|Inside-Spread Spoofing]]
- [[Cases/CASE-051|Trash and Cash]]

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
