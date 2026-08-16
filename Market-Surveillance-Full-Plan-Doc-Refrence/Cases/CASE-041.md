---
id: CASE-041
type: surveillance-case
case_number: 41
title: "Group / Syndicate Manipulation"
status: implementation-seeded
implementation_archetype: communications
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 41. Group / Syndicate Manipulation

## Description

A larger group coordinates purchases, sales, orders, or promotions to influence a stock.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]
- [[Families/FAMILY-21|Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-19|Position Concentration]]

## Related cases

- [[Cases/CASE-464|Investment-Group Signal Scam Trading]]
- [[Cases/CASE-264|Hidden Beneficial-Ownership Group]]
- [[Cases/CASE-149|Reverse/Forward Split Share-Concentration Scheme]]
- [[Cases/CASE-244|Returned IPO Share Premium Capture]]
- [[Cases/CASE-095|Manipulative Short Selling]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

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
