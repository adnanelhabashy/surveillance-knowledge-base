---
id: CASE-483
type: surveillance-case
case_number: 483
title: "Affiliate Internalization Favoritism"
status: implementation-seeded
implementation_archetype: routing_venue
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 483. Affiliate Internalization Favoritism

## Description

Preferentially matching customer flow against an affiliate or proprietary account despite conflicts or inferior outcomes.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]
- [[Families/FAMILY-16|Dark-pool, ATS, internalization & venue-conflict abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]

## Related cases

- [[Cases/CASE-247|Affiliated-Venue Routing Conflict]]
- [[Cases/CASE-479|Venue-Operator Proprietary Front Running]]
- [[Cases/CASE-270|Internalization Profit at Customer Expense]]
- [[Cases/CASE-346|Affiliate Wash Trading]]
- [[Cases/CASE-482|Broker Crossing-Engine Favoritism]]

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
