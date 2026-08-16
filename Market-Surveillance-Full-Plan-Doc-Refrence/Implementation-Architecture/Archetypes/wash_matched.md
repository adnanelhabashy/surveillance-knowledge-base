---
id: ARCHETYPE-WASH_MATCHED
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Wash / matched / self / circular trading

- **Catalog cases:** 29
- **Primary state owner:** `ParticipantInstrumentGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Optional later for unknown coordination clusters; not required for known-rule detection.

## Grain set

- `ParticipantInstrumentGrain`
- `AccountGrain`
- `BeneficialOwnerGrain`
- `RelationshipGrain`
- `CoordinationWindowGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `RelationshipEvent`
- `AccountReference`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-003|CASE-003 — Wash Trading / Wash Sales]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-004|CASE-004 — Matched Orders / Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-005|CASE-005 — Prearranged Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-006|CASE-006 — Circular Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-007|CASE-007 — Self-Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-035|CASE-035 — Crossing Manipulation]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-126|CASE-126 — Pump + Matched Orders]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Price Impact
- [[Cases/CASE-190|CASE-190 — Liquidity-Rebate Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-340|CASE-340 — Profit-Transfer Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-341|CASE-341 — Loss-Transfer Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-344|CASE-344 — Multi-Venue Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-345|CASE-345 — Cross-Product Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Cross-Product Economic Benefit
- [[Cases/CASE-346|CASE-346 — Affiliate Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-347|CASE-347 — Omnibus Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-348|CASE-348 — Split-Quantity Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-349|CASE-349 — Staggered Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-350|CASE-350 — Intermediated Matched Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-351|CASE-351 — Variable-Quantity Circular Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Related-Account Graph
- [[Cases/CASE-352|CASE-352 — Cross-Venue Circular Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Cross-Venue Synchronization
- [[Cases/CASE-353|CASE-353 — Synthetic Round-Trip Trading]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation
- [[Cases/CASE-354|CASE-354 — Reverse Round-Trip Trading]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-355|CASE-355 — Closing-Print Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Auction Indicative-Price Impact
- [[Cases/CASE-356|CASE-356 — Opening-Print Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Auction Indicative-Price Impact
- [[Cases/CASE-357|CASE-357 — Benchmark-Window Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Benchmark-Window Participation
- [[Cases/CASE-379|CASE-379 — Settlement-Window Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph, Benchmark-Window Participation
- [[Cases/CASE-468|CASE-468 — Rebate-Farming Self-Match]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-469|CASE-469 — Fee-Tier Volume Inflation]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal
- [[Cases/CASE-470|CASE-470 — Market-Share Incentive Wash Trading]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph
- [[Cases/CASE-522|CASE-522 — Prearranged Block-Trade Abuse]] — detectors: Self / Related Beneficial Owner, Time / Price / Quantity Matching, Circular Transaction Graph

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
