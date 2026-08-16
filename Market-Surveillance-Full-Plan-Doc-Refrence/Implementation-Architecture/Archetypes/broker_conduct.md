---
id: ARCHETYPE-BROKER_CONDUCT
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Broker/customer trading conduct

- **Catalog cases:** 21
- **Primary state owner:** `AccountGrain`
- **Primary services:** Client Order Adapter, Broker Records Adapter, Live Orleans Cluster
- **AI boundary:** Not required.

## Grain set

- `AccountGrain`
- `TraderGrain`
- `ClientOrderWindowGrain`
- `PositionGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ClientOrderEvent`
- `ExecutionEvent`
- `AccountAuthorizationEvent`
- `FeeEvent`
- `PositionEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-099|CASE-099 — Churning]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-101|CASE-101 — Misappropriation of Customer Assets]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-105|CASE-105 — Excessive Markup / Markdown Fraud]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-106|CASE-106 — Commission / Fee Manipulation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-145|CASE-145 — Cash-Account Free-Riding]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-186|CASE-186 — Deceptive Mutual-Fund Market Timing]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-222|CASE-222 — Performance-Fee Inflation Through False Valuation]] — detectors: Benchmark-Window Participation, Price Impact, Volume Participation
- [[Cases/CASE-224|CASE-224 — Unfair Client Cross-Trade Pricing]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-225|CASE-225 — Personal-Account Cross-Trade Abuse]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-226|CASE-226 — Riskless-Principal Compensation Concealment]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Time / Price / Quantity Matching
- [[Cases/CASE-227|CASE-227 — Error-Account Abuse]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-229|CASE-229 — Cancel-and-Rebill Manipulation]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-230|CASE-230 — Selling Away]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-248|CASE-248 — Undisclosed Principal Trading with Advisory Clients]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Time / Price / Quantity Matching
- [[Cases/CASE-249|CASE-249 — Improper Agency-Cross Transaction]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-250|CASE-250 — Wrap-Fee Trading-Away Cost Concealment]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-260|CASE-260 — Interfund Cross-Trade Conflict Abuse]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-261|CASE-261 — Interfund Cross-Trade Mispricing]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-292|CASE-292 — Customer Reserve Requirement Manipulation Through Affiliates]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-342|CASE-342 — Client-to-Proprietary Value Transfer]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact
- [[Cases/CASE-343|CASE-343 — Proprietary-to-Favored-Client Value Transfer]] — detectors: Time / Price / Quantity Matching, Trade-Report Timing / Accuracy, Price Impact

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
