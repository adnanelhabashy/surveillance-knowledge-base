---
id: CASE-200
type: surveillance-case
case_number: 200
title: "Distribution Restricted-Period Manipulation"
status: implementation-seeded
implementation_archetype: offering
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 200. Distribution Restricted-Period Manipulation

## Description

An issuer, underwriter, or distribution participant improperly bids for or purchases a security during a restricted offering period to influence market price.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]
- [[Families/FAMILY-23|IPO, new-issue, distribution & stabilization trading abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-164|Artificial Aftermarket Conditioning During a Distribution]]
- [[Cases/CASE-240|Restricted-Person New-Issue Allocation]]
- [[Cases/CASE-161|Pre-Offering Short-Sale / Rule 105 Abuse]]
- [[Cases/CASE-160|Alternative Merger / Exchange-Offer Price Manipulation]]
- [[Cases/CASE-189|Improper Price Stabilization]]

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
