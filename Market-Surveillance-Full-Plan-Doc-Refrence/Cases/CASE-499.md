---
id: CASE-499
type: surveillance-case
case_number: 499
title: "Out-of-Sequence Print Manipulation"
status: implementation-seeded
implementation_archetype: reporting
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 499. Out-of-Sequence Print Manipulation

## Description

Reporting trades out of sequence with manipulative intent so the tape presents a misleading chronology or price trend.

## Surveillance families

- [[Families/FAMILY-04|Price, volume & tape manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-334|Sequence Painting]]
- [[Cases/CASE-008|Painting the Tape]]
- [[Cases/CASE-335|Alternating Print Manipulation]]
- [[Cases/CASE-188|Issuer Share-Repurchase Manipulation]]
- [[Cases/CASE-354|Reverse Round-Trip Trading]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic reconciliation model
- **Detection mode:** Rules; AI not required
- **Rule logic (starter):** Flag differences between source executions/orders and published/regulatory/customer reports, including false, delayed, missing or altered price, quantity, venue, capacity, time or transaction identity.
- **Orleans grains/state:** TradeGrain, ReportingGrain, VenueGrain, AccountGrain, SurveillanceGrain; retain immutable source execution and every report/amendment
- **Required event fields:** sourceTradeId, reportId, executionTime, reportTime, instrumentId, price, quantity, buyer/seller/account IDs, venue, capacity, tradeCondition, correction/cancel flags, publication status
- **Time window(s):** Immediate reconciliation; reporting-deadline window; end-of-day completeness check; rolling 30-day participant error pattern
- **Thresholds/calibration:** Hard alert for fictitious/missing source trade, material price/quantity/identity mismatch, or report later than applicable configured deadline. Escalate repeated mismatches ≥ 3/day or error rate > 99th-percentile peer baseline.
- **Alert evidence:** Original execution record; submitted report versions; field-level diff; timestamps/deadline; downstream publication; participant correction history
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
