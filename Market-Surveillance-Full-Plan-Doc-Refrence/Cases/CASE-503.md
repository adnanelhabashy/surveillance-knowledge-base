---
id: CASE-503
type: surveillance-case
case_number: 503
title: "Pump-and-Dump Reporting Delay"
status: implementation-seeded
implementation_archetype: reporting
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 503. Pump-and-Dump Reporting Delay

## Description

Delaying or sequencing trade reports so the manipulator's distribution is less visible during a pump-and-dump.

## Surveillance families

- [[Families/FAMILY-07|Momentum ignition, ramping, pumping & dumping]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-456|Nominee-Account Pump]]
- [[Cases/CASE-126|Pump + Matched Orders]]
- [[Cases/CASE-048|Pump and Dump]]
- [[Cases/CASE-457|Pump-and-Dump with Options]]
- [[Cases/CASE-458|Pump-and-Dump with Warrants]]

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
