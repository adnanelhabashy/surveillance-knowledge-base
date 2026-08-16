---
id: CASE-282
type: surveillance-case
case_number: 282
title: "Selective Advance Distribution of Research"
status: implementation-seeded
implementation_archetype: offering
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 282. Selective Advance Distribution of Research

## Description

A research report or recommendation is provided to selected trading personnel or favored customers before other customers entitled to receive it.

## Surveillance families

- [[Families/FAMILY-23|IPO, new-issue, distribution & stabilization trading abuse]]

## Reusable detector starting points

- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-09|Price Impact]]

## Related cases

- [[Cases/CASE-067|Front Running Research]]
- [[Cases/CASE-239|IPO / New-Issue Withholding for Firm or Insider Benefit]]
- [[Cases/CASE-076|Misleading Analyst Research]]
- [[Cases/CASE-200|Distribution Restricted-Period Manipulation]]
- [[Cases/CASE-164|Artificial Aftermarket Conditioning During a Distribution]]

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
