---
id: CASE-482
type: surveillance-case
case_number: 482
title: "Broker Crossing-Engine Favoritism"
status: implementation-seeded
implementation_archetype: routing_venue
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 482. Broker Crossing-Engine Favoritism

## Description

Operating a crossing system so a proprietary or favored counterparty receives systematically advantageous matches.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-035|Crossing Manipulation]]
- [[Cases/CASE-275|Hidden Affiliate Counterparty Dominance]]
- [[Cases/CASE-483|Affiliate Internalization Favoritism]]
- [[Cases/CASE-040|Broker–Client Collusion]]
- [[Cases/CASE-342|Client-to-Proprietary Value Transfer]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + routing/venue configuration and fee data; AI not required
- **Rule logic (starter):** Flag routing/execution patterns where customer outcomes are systematically worse while the broker/venue/affiliate receives economic benefit, or where venue functionality/data is selectively provided or concealed.
- **Orleans grains/state:** OrderRoutingGrain, ExecutionQualityGrain, VenueGrain, AccountGrain, RelationshipGrain, SurveillanceGrain; keep route decisions, NBBO/quotes, fees/rebates, affiliate status and client outcomes
- **Required event fields:** orderId, decisionTime, accountId, instrumentId, side, size, routeVenue, availableVenues/quotes, NBBO, executionPrice/time, fillRate, fee/rebate/PFOF, affiliate flag, orderType/functionality access, customer report venue
- **Time window(s):** Per-order; rolling 5–30 min market condition window; daily/weekly routing-quality aggregates
- **Thresholds/calibration:** Start with systematic price disadvantage ≥ 1 tick versus feasible alternative, materially lower fill rate/latency, affiliate/PFOF concentration > peer/baseline, or selective-function access not disclosed. Require repeated pattern, e.g. ≥ 20 orders or statistically significant sample.
- **Alert evidence:** Routing decision and alternatives; contemporaneous quotes; execution markout; fees/rebates; affiliate relationship; customer disclosure/configuration; aggregate comparison by venue
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
