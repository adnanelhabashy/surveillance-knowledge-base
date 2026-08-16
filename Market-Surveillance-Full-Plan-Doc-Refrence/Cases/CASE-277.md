---
id: CASE-277
type: surveillance-case
case_number: 277
title: "Undisclosed Preferential ATS Order Type"
status: implementation-seeded
implementation_archetype: routing_venue
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 277. Undisclosed Preferential ATS Order Type

## Description

A trading venue gives certain participants access to an advantageous order type without adequately disclosing that functionality to all subscribers.

## Surveillance families

- [[Families/FAMILY-05|Opening, closing & auction manipulation]]
- [[Families/FAMILY-16|Dark-pool, ATS, internalization & venue-conflict abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]]
- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]

## Related cases

- [[Cases/CASE-279|Selective ATS Functionality Access]]
- [[Cases/CASE-274|Secret Proprietary Desk Inside an ATS]]
- [[Cases/CASE-480|Selective Latency Advantage Abuse]]
- [[Cases/CASE-538|Order-Type Exploitation Manipulation]]
- [[Cases/CASE-059|Undisclosed Touting]]

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
