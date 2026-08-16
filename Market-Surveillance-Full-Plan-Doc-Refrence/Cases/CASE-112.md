---
id: CASE-112
type: surveillance-case
case_number: 112
title: "IPO Allocation / Spinning Abuse"
status: implementation-seeded
implementation_archetype: offering
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 112. IPO Allocation / Spinning Abuse

## Description

Attractive IPO allocations are improperly provided to executives or decision-makers to influence business relationships.

## Surveillance families

- [[Families/FAMILY-23|IPO, new-issue, distribution & stabilization trading abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-163|IPO Allocation-for-Excessive-Commission Scheme]]
- [[Cases/CASE-162|IPO Aftermarket Tie-In Scheme]]
- [[Cases/CASE-241|IPO Flipping-Penalty Recoupment Abuse]]
- [[Cases/CASE-244|Returned IPO Share Premium Capture]]
- [[Cases/CASE-242|Undisclosed IPO Lock-Up Release / Waiver]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + offering/tender reference data; AI not required
- **Rule logic (starter):** Flag trading or allocation behavior during offering/tender/distribution restricted periods that conflicts with configured eligibility, allocation, purchase, short-sale, stabilization or equal-consideration constraints, or artificially conditions aftermarket demand.
- **Orleans grains/state:** OfferingGrain, AccountGrain, InvestorGrain, PositionGrain, OrderBookGrain, RelationshipGrain, SurveillanceGrain; store restricted periods, allocations, participant roles and tender eligibility
- **Required event fields:** offeringId/tenderId, instrumentId, participant/account role, restrictedPerson flag, periodStart/End, allocationQty/price, orders/trades, short position, tenderQty, netLongQty, stabilization flag, outsidePurchase terms, lockup/release data
- **Time window(s):** Configured offering/tender/restricted period; 5–20 trading days pre-offer where relevant; first 1–10 aftermarket days
- **Thresholds/calibration:** Use hard eligibility/period constraints where rules are explicit. Behavioral starter: aftermarket participation or linked-account buying > 20% of volume, tenderQty > netLongQty, or transaction terms more favorable than configured offer terms.
- **Alert evidence:** Offering/tender terms; participant role; allocations; restricted-window orders/trades; net position; stabilization/exception flags; linked-account activity; aftermarket price/volume effect
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
