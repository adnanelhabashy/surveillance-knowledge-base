---
id: CASE-460
type: surveillance-case
case_number: 460
title: "Pump Before Insider Sale"
status: implementation-seeded
implementation_archetype: insider
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 460. Pump Before Insider Sale

## Description

A stock is manipulated upward before a controlling holder or insider sells a significant position.

## Surveillance families

- [[Families/FAMILY-07|Momentum ignition, ramping, pumping & dumping]]
- [[Families/FAMILY-11|Insider dealing & misuse of material non-public information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-459|Pump Before Financing]]
- [[Cases/CASE-061|Insider Tipping]]
- [[Cases/CASE-060|Insider Trading / Insider Dealing]]
- [[Cases/CASE-048|Pump and Dump]]
- [[Cases/CASE-146|Restricted-Stock Loan Default Scheme]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + external/reference data; AI not required
- **Rule logic (starter):** Flag statistically unusual trading, order amendment/cancellation or recommendations by insiders/connected persons before a material event, especially when direction and eventual profit align with the event. This is a suspicion rule, not proof of possession of MNPI.
- **Orleans grains/state:** InvestorGrain, AccountGrain, RelationshipGrain, PositionGrain, CorporateEventGrain, TraderGrain, SurveillanceGrain; store insider relationships, event timeline and participant historical baselines
- **Required event fields:** eventTime, investor/account/trader IDs, instrumentId, side, price, quantity, order action, positionBefore/After, insider/relationship type, materialEventId, eventType, eventAnnouncementTime, public/non-public status where available, realized/unrealized P&L
- **Time window(s):** 1, 5, 10 and 30 trading days before material event; immediate order amend/cancel after information-access timestamp when available; post-event 1–5 day profit window
- **Thresholds/calibration:** Start with trade size/position change > 95th percentile of participant history, abnormal directional exposure, event proximity ≤ 5 trading days for high priority, and post-event move/P&L materially favorable. Calibrate by event type and normal trading pattern.
- **Alert evidence:** Relationship to issuer/event; exact trade/order timeline; event timeline; historical trading baseline; position change; abnormal return/P&L; communications/access logs if later available
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
