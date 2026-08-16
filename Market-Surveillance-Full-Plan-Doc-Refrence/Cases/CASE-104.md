---
id: CASE-104
type: surveillance-case
case_number: 104
title: "Interpositioning Abuse"
status: implementation-seeded
implementation_archetype: routing_venue
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 104. Interpositioning Abuse

## Description

An unnecessary intermediary is inserted into a transaction to generate hidden profits or disadvantage the customer.

## Surveillance families

- [[Families/FAMILY-25|Client/proprietary cross-trade and execution-value-transfer abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-423|Misuse of Customer IOI Information]]
- [[Cases/CASE-105|Excessive Markup / Markdown Fraud]]
- [[Cases/CASE-225|Personal-Account Cross-Trade Abuse]]
- [[Cases/CASE-044|Cross-Market Manipulation]]
- [[Cases/CASE-228|Margin-Evasion Through Error Accounts]]

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
