---
id: CASE-062
type: surveillance-case
case_number: 62
title: "Tippee Trading"
status: implementation-seeded
implementation_archetype: insider
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 62. Tippee Trading

## Description

A recipient of improperly disclosed inside information trades using that information.

## Surveillance families

- [[Families/FAMILY-05|Opening, closing & auction manipulation]]
- [[Families/FAMILY-11|Insider dealing & misuse of material non-public information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-179|Inside-Information Recommendation / Inducement]]
- [[Cases/CASE-127|Insider Trading + Market Manipulation]]
- [[Cases/CASE-182|Cyber-Hacked MNPI Trading]]
- [[Cases/CASE-080|Delayed Material Disclosure Abuse]]
- [[Cases/CASE-180|Wall-Cross / Market-Sounding Insider Trading]]

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
