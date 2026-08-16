---
id: CASE-128
type: surveillance-case
case_number: 128
title: "Rumor + Trading Manipulation"
status: implementation-seeded
implementation_archetype: communications
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 128. Rumor + Trading Manipulation

## Description

A trader establishes a position, spreads false information, and trades around the resulting price movement.

## Surveillance families

- [[Families/FAMILY-02|Order-book pressure & quotation manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-20|Liquidity Concentration]]

## Related cases

- [[Cases/CASE-050|Short and Distort]]
- [[Cases/CASE-075|False Social-Media Information]]
- [[Cases/CASE-135|Phishing / Order-Book Phishing]]
- [[Cases/CASE-288|Securities-Loan Rate / Fee Misreporting]]
- [[Cases/CASE-472|Coordinated Quote Widening]]

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
