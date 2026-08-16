---
id: ARCHETYPE-FRONT_RUN
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Front running / trading ahead / client-order misuse

- **Catalog cases:** 29
- **Primary state owner:** `ClientOrderWindowGrain`
- **Primary services:** Client Order Adapter, Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `ClientOrderWindowGrain`
- `ParticipantInstrumentGrain`
- `TraderGrain`
- `AccountGrain`
- `RoutingQualityGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ClientOrderEvent`
- `ExecutionEvent`
- `OrderEvent`
- `RoutingDecisionEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-064|CASE-064 — Trading Ahead]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-065|CASE-065 — Front Running]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-066|CASE-066 — Front Running Block Orders]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-068|CASE-068 — Front Running Index Changes]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-069|CASE-069 — Front Running Corporate Actions]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact, Benchmark-Window Participation
- [[Cases/CASE-070|CASE-070 — Misuse of Client Order Information]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-187|CASE-187 — Selective Portfolio-Holdings Abuse]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-231|CASE-231 — Piggybacking / Shadowing Customer Trades]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-408|CASE-408 — Front Running Customer Limit Orders]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-409|CASE-409 — Front Running Stop Orders]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-410|CASE-410 — Front Running Market-on-Close Orders]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-411|CASE-411 — Front Running VWAP Orders]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-412|CASE-412 — Front Running Algorithmic Parent Orders]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact, Order-Message Burst Rate
- [[Cases/CASE-413|CASE-413 — Front Running Iceberg Orders]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-414|CASE-414 — RFQ Front Running]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-415|CASE-415 — RFQ Information Leakage Abuse]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-416|CASE-416 — Abusive Pre-Hedging]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-417|CASE-417 — Front Running Block Crosses]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-418|CASE-418 — Front Running Offerings]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-419|CASE-419 — Front Running Tender Activity]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact, Benchmark-Window Participation
- [[Cases/CASE-420|CASE-420 — Front Running Benchmark Fixes]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-422|CASE-422 — Trading with Knowledge of Order Imbalance]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-423|CASE-423 — Misuse of Customer IOI Information]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-424|CASE-424 — Misuse of Portfolio-Composition Files]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-425|CASE-425 — Misuse of Stop-Level Information]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-477|CASE-477 — Conditional-Order Information Abuse]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-478|CASE-478 — IOI Leakage Trading Abuse]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact
- [[Cases/CASE-479|CASE-479 — Venue-Operator Proprietary Front Running]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching, Pre-Event Abnormal Trading
- [[Cases/CASE-523|CASE-523 — Block-Trade Front Running]] — detectors: Pre-Event Abnormal Trading, Time / Price / Quantity Matching, Price Impact

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
