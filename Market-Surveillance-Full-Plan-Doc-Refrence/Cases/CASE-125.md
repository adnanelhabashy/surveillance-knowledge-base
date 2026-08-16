---
id: CASE-125
type: surveillance-case
case_number: 125
title: "Pump + Wash Trading"
status: implementation-seeded
implementation_archetype: communications
smarts_public_mapping: variant-of-publicly-described-behavior
tags:
  - surveillance/case
---

# 125. Pump + Wash Trading

## Description

Wash trades manufacture apparent activity while promotion attracts genuine investors.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]
- [[Families/FAMILY-07|Momentum ignition, ramping, pumping & dumping]]
- [[Families/FAMILY-21|Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]

## Related cases

- [[Cases/CASE-346|Affiliate Wash Trading]]
- [[Cases/CASE-003|Wash Trading / Wash Sales]]
- [[Cases/CASE-190|Liquidity-Rebate Wash Trading]]
- [[Cases/CASE-355|Closing-Print Wash Trading]]
- [[Cases/CASE-356|Opening-Print Wash Trading]]

## SMARTS mapping

- **Public mapping:** This is a narrower variant of a behavior Nasdaq publicly describes SMARTS as monitoring.
- Nasdaq does not publish the full proprietary alert library, so this note does not claim a one-to-one SMARTS alert.

## Implementation workspace

- **Rule status:** Hybrid model: trading rule can be modeled now
- **Detection mode:** Rules + external communications/news data; AI helpful for text meaning, not required for trade-pattern leg
- **Rule logic (starter):** Flag suspicious trading around promotional, false/misleading, rumor or recommendation activity by linking communication timestamps/entities to accumulation before publication and distribution/short-covering after market reaction. Trading leg can be rules-based; content interpretation requires external text and may benefit from NLP/AI.
- **Orleans grains/state:** TraderGrain, InvestorGrain, AccountGrain, InstrumentGrain, PositionGrain, CommunicationSignalGrain/ExternalEventGrain, SurveillanceGrain; retain author/entity links and pre/post communication positions
- **Required event fields:** communicationId, publishTime, author/source, mentionedInstrument/entity, extracted sentiment/claim if available, trader/investor/account IDs, order/trade timestamps, side, price, quantity, positionBefore/After, volume/price response, related-party mapping
- **Time window(s):** Accumulation 1–20 trading days before communication; reaction 0–60 min and same day; distribution/covering 0–5 trading days after; longer baseline 20–60 days
- **Thresholds/calibration:** Trading starter: pre-event accumulation > 95th percentile, post-message volume > 3× baseline or price move > 3%, promoter/related account sells/covers ≥ 20% of position into reaction. Text-risk thresholds should be calibrated separately.
- **Alert evidence:** Communication snapshot/hash and timestamp; author/entity relationship; pre-event positions; market reaction; promoter/related trading; realized P&L; repeated campaigns
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
