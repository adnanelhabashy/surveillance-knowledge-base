---
id: CASE-049
type: surveillance-case
case_number: 49
title: "Ramp and Dump"
status: implementation-seeded
implementation_archetype: communications
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 49. Ramp and Dump

## Description

Manipulative trading rather than promotion alone is used to ramp the price upward before selling the position.

## Surveillance families

- [[Families/FAMILY-07|Momentum ignition, ramping, pumping & dumping]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]

## Related cases

- [[Cases/CASE-458|Pump-and-Dump with Warrants]]
- [[Cases/CASE-457|Pump-and-Dump with Options]]
- [[Cases/CASE-454|Closing-Price Pump]]
- [[Cases/CASE-048|Pump and Dump]]
- [[Cases/CASE-503|Pump-and-Dump Reporting Delay]]

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
