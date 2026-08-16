---
id: ARCHETYPE-COMMUNICATIONS
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Promotion / rumor / communications-linked trading

- **Catalog cases:** 39
- **Primary state owner:** `ParticipantInstrumentGrain`
- **Primary services:** Live Stream Processor, External Signal Adapter (optional now), Live Orleans Cluster
- **AI boundary:** Future AI is useful for text meaning and communications analysis. Trade-side detection remains deterministic.

## Grain set

- `ParticipantInstrumentGrain`
- `PositionGrain`
- `RelationshipGrain`
- `CorporateEventGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `OrderEvent`
- `PromotionSignal`
- `NewsEvent`
- `RelationshipEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-041|CASE-041 — Group / Syndicate Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Volume Participation
- [[Cases/CASE-049|CASE-049 — Ramp and Dump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-050|CASE-050 — Short and Distort]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-051|CASE-051 — Trash and Cash]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-055|CASE-055 — Pump-and-Dump Through Social Media]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-056|CASE-056 — Chat-Room Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-057|CASE-057 — Influencer Stock Manipulation]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-058|CASE-058 — Scalping Scheme]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-059|CASE-059 — Undisclosed Touting]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Position Concentration
- [[Cases/CASE-067|CASE-067 — Front Running Research]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact, Trade-Report Timing / Accuracy
- [[Cases/CASE-072|CASE-072 — False-Rumor Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-073|CASE-073 — Misleading-News Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-074|CASE-074 — Fake Press Release Fraud]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-075|CASE-075 — False Social-Media Information]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-076|CASE-076 — Misleading Analyst Research]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-077|CASE-077 — Undisclosed Research Conflict]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-078|CASE-078 — False Corporate Disclosure]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-079|CASE-079 — Material-Omission Fraud]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-080|CASE-080 — Delayed Material Disclosure Abuse]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-125|CASE-125 — Pump + Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Price Impact
- [[Cases/CASE-128|CASE-128 — Rumor + Trading Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-148|CASE-148 — Dormant Shell Hijacking]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-150|CASE-150 — Trend-Hijacking Issuer Promotion]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-151|CASE-151 — Unsupported Partnership / Joint-Venture Promotion]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-152|CASE-152 — Dormancy-Reactivation Promotion Scheme]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-166|CASE-166 — Stock-Promoter Liquidation Scheme]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-167|CASE-167 — Quotation-Reactivation Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-199|CASE-199 — Imposter Market-Information Source Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-207|CASE-207 — Direct-To-Investor Stock-Manipulation Scam]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-216|CASE-216 — Paid Market-Making Influence]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Displayed-Size Anomaly
- [[Cases/CASE-217|CASE-217 — Paid Publication to Influence Stock Price]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-283|CASE-283 — Analyst Trading Against Own Recommendation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-284|CASE-284 — Research-Analyst Blackout Trading]] — detectors: Trade-Report Timing / Accuracy, Time / Price / Quantity Matching
- [[Cases/CASE-451|CASE-451 — Accumulation-Promotion-Distribution Scheme]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Volume Participation
- [[Cases/CASE-454|CASE-454 — Closing-Price Pump]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-457|CASE-457 — Pump-and-Dump with Options]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Cross-Product Economic Benefit
- [[Cases/CASE-458|CASE-458 — Pump-and-Dump with Warrants]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-464|CASE-464 — Investment-Group Signal Scam Trading]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-467|CASE-467 — Promotion-Triggered Liquidity Exit]] — detectors: Volume Participation, Position Concentration, Related-Account Graph

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
