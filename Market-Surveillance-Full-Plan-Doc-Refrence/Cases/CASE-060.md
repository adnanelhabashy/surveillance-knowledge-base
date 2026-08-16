---
id: CASE-060
type: surveillance-case
case_number: 60
title: "Insider Trading / Insider Dealing"
status: implementation-seeded
implementation_archetype: insider
smarts_public_mapping: explicitly-publicly-described
tags:
  - surveillance/case
---

# 60. Insider Trading / Insider Dealing

## Description

A person trades while possessing material non-public information.

## Surveillance families

- [[Families/FAMILY-11|Insider dealing & misuse of material non-public information]]

## Reusable detector starting points

- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-429|Insider Trading Before Buyback Announcement]]
- [[Cases/CASE-061|Insider Tipping]]
- [[Cases/CASE-430|Insider Trading Before Capital Raise]]
- [[Cases/CASE-427|Insider Trading Before Earnings]]
- [[Cases/CASE-184|Rule 10b5-1 Trading-Plan Abuse]]

## SMARTS mapping

- **Public mapping:** Nasdaq SMARTS materials explicitly describe **Insider trading** surveillance/capability.
- This does **not** imply the public name equals a proprietary SMARTS alert name.

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
