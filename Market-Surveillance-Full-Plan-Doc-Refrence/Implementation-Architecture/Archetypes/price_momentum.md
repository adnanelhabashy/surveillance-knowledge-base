---
id: ARCHETYPE-PRICE_MOMENTUM
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Price / momentum / tape manipulation

- **Catalog cases:** 39
- **Primary state owner:** `InstrumentGrain`
- **Primary services:** Live Stream Processor, Live Orleans Cluster
- **AI boundary:** Not required for core detection.

## Grain set

- `InstrumentGrain`
- `OrderBookGrain`
- `ParticipantInstrumentGrain`
- `PositionGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `OrderEvent`
- `MarketStateEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-013|CASE-013 — Ramping]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-014|CASE-014 — Price Ramping / Price Run-Up]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-015|CASE-015 — Price Depressing]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-016|CASE-016 — Momentum Ignition]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Order-Message Burst Rate
- [[Cases/CASE-022|CASE-022 — Price Flooring]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-023|CASE-023 — Artificial Price Maintenance]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-048|CASE-048 — Pump and Dump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Position Concentration
- [[Cases/CASE-053|CASE-053 — Microcap / Penny-Stock Manipulation]] — detectors: Volume Participation, Position Concentration, Related-Account Graph
- [[Cases/CASE-139|CASE-139 — Illiquid Price-Setting Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-140|CASE-140 — Short-Window Price Spike-and-Reversal Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-141|CASE-141 — Rapid Position-Reversal Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-144|CASE-144 — Advancing the Bid]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-153|CASE-153 — Small-Purchase Price Support for Large Inventory]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-188|CASE-188 — Issuer Share-Repurchase Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-193|CASE-193 — Engineered Short Squeeze]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-255|CASE-255 — Stop-Loss Trigger Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-304|CASE-304 — Liquidity-Vacuum Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-314|CASE-314 — Odd-Lot Last-Sale Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-330|CASE-330 — Last-Sale Price Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-331|CASE-331 — Intraday High Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-332|CASE-332 — Intraday Low Marking]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-333|CASE-333 — Microtrade Price Setting]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-334|CASE-334 — Sequence Painting]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-337|CASE-337 — Trade-Cluster Momentum Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-338|CASE-338 — Quote-and-Trade Combination Manipulation]] — detectors: Displayed-Size Anomaly, Multi-Level Depth Pressure, Price Impact, Liquidity Concentration
- [[Cases/CASE-362|CASE-362 — Banging the Close]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-365|CASE-365 — Quarter-End Marking]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-366|CASE-366 — Year-End Marking]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-367|CASE-367 — Index-Rebalance Close Manipulation]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Benchmark-Window Participation
- [[Cases/CASE-369|CASE-369 — Pre-Close Ramp-and-Reverse]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation
- [[Cases/CASE-455|CASE-455 — Low-Float Squeeze Pump]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Position Concentration
- [[Cases/CASE-459|CASE-459 — Pump Before Financing]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-461|CASE-461 — Pump Before Share Issuance]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Pre-Event Abnormal Trading
- [[Cases/CASE-463|CASE-463 — Pump-and-Reload Cycle]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-466|CASE-466 — Victim-Order Price Support]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-526|CASE-526 — Free-Float Corner]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-527|CASE-527 — Record-Date Squeeze]] — detectors: Position Concentration, Liquidity Concentration, Price Impact
- [[Cases/CASE-528|CASE-528 — Ex-Date Price Manipulation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Pre-Event Abnormal Trading
- [[Cases/CASE-532|CASE-532 — Algorithmic Momentum Ignition]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Order-Message Burst Rate

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
