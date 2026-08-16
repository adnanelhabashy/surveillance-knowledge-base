---
id: ARCHETYPE-AUCTION_CLOSE
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Opening / closing / auction manipulation

- **Catalog cases:** 22
- **Primary state owner:** `AuctionGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `AuctionGrain`
- `OrderBookGrain`
- `InstrumentGrain`
- `ParticipantInstrumentGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `AuctionEvent`
- `OrderEvent`
- `ExecutionEvent`
- `MarketPhaseEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-009|CASE-009 — Marking the Close]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-010|CASE-010 — Marking the Open]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-011|CASE-011 — Closing-Auction Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-012|CASE-012 — Opening-Auction Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-130|CASE-130 — Closing-Price + Derivative Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Cross-Product Economic Benefit
- [[Cases/CASE-142|CASE-142 — Portfolio Pumping]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-143|CASE-143 — Window Dressing]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-171|CASE-171 — Auction Uncrossing-Volume Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Auction Indicative-Price Impact
- [[Cases/CASE-210|CASE-210 — Closing-Bid Marking]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-243|CASE-243 — Pre-Opening IPO Market-Order Abuse]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-363|CASE-363 — Banging the Open]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-370|CASE-370 — Closing-Auction Order Flooding]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-371|CASE-371 — Opening-Auction Order Flooding]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-372|CASE-372 — Late Auction Cancellation Abuse]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-373|CASE-373 — Auction-Extension Triggering Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-374|CASE-374 — Auction-Extension Avoidance Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-375|CASE-375 — Auction-Price-Collar Gaming]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Order-Message Burst Rate
- [[Cases/CASE-377|CASE-377 — Opening-Cross Participation Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-400|CASE-400 — Auction-to-Continuous Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-401|CASE-401 — Continuous-to-Auction Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-421|CASE-421 — Trading with Knowledge of Auction Imbalance]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-453|CASE-453 — Opening-Gap Pump]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
