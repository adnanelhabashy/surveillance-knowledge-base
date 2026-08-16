---
id: CASE-133
type: surveillance-case
case_number: 133
title: "Event-Driven Manipulation"
status: implementation-seeded
implementation_archetype: offering
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 133. Event-Driven Manipulation

## Description

Manipulative trading is concentrated around corporate announcements, index changes, dividends, rights issues, offerings, or other known market events.

## Surveillance families

- [[Families/FAMILY-10|Cross-product, derivatives, ETF/ETP & index manipulation]]
- [[Families/FAMILY-23|IPO, new-issue, distribution & stabilization trading abuse]]
- [[Families/FAMILY-24|Tender, corporate-action & record-date trading manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-12|Benchmark-Window Participation]]

## Related cases

- [[Cases/CASE-068|Front Running Index Changes]]
- [[Cases/CASE-069|Front Running Corporate Actions]]
- [[Cases/CASE-084|Index-Level Manipulation]]
- [[Cases/CASE-177|Rights-Issue Reference-Price Manipulation]]
- [[Cases/CASE-394|Rights-to-Underlying Manipulation]]

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
