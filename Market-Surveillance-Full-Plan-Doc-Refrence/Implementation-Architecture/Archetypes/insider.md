---
id: ARCHETYPE-INSIDER
type: implementation-archetype
status: reference
tags:
  - surveillance/implementation
---


# Archetype — Insider dealing / MNPI-linked trading

- **Catalog cases:** 25
- **Primary state owner:** `CorporateEventGrain`
- **Primary services:** Live Stream Processor, Reference Data Sync, Live Orleans Cluster
- **AI boundary:** Future NLP can enrich event/news understanding; deterministic trade-event correlation remains the core.

## Grain set

- `CorporateEventGrain`
- `RelationshipGrain`
- `BeneficialOwnerGrain`
- `ParticipantInstrumentGrain`
- `PositionGrain`
- `RuleEvaluationWorkerGrain`

## Required data domains

- `ExecutionEvent`
- `CorporateEvent`
- `InsiderRelationshipEvent`
- `HoldingEvent`

## Implementation pattern

1. Route only matching event/fact types into this archetype.
2. Let the state-owner grain update ordered state and bounded windows.
3. Run reusable detectors against the updated state.
4. Produce an immutable fact bundle.
5. Evaluate only this archetype's active rule pack.
6. Correlate/deduplicate resulting alerts and emit evidence references.

## Cases

- [[Cases/CASE-060|CASE-060 — Insider Trading / Insider Dealing]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-061|CASE-061 — Insider Tipping]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-062|CASE-062 — Tippee Trading]] — detectors: Auction Indicative-Price Impact, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-063|CASE-063 — Information Misappropriation]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-071|CASE-071 — Selective Disclosure Abuse]] — detectors: Self / Related Beneficial Owner, Related-Account Graph, Time / Price / Quantity Matching
- [[Cases/CASE-127|CASE-127 — Insider Trading + Market Manipulation]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-146|CASE-146 — Restricted-Stock Loan Default Scheme]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph, Short / Borrow / Settlement Status, Position Concentration
- [[Cases/CASE-178|CASE-178 — Insider Order Amendment / Cancellation]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-179|CASE-179 — Inside-Information Recommendation / Inducement]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-180|CASE-180 — Wall-Cross / Market-Sounding Insider Trading]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-181|CASE-181 — Shadow Trading]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-182|CASE-182 — Cyber-Hacked MNPI Trading]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-184|CASE-184 — Rule 10b5-1 Trading-Plan Abuse]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-239|CASE-239 — IPO / New-Issue Withholding for Firm or Insider Benefit]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-240|CASE-240 — Restricted-Person New-Issue Allocation]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-242|CASE-242 — Undisclosed IPO Lock-Up Release / Waiver]] — detectors: Volume Participation, Pre-Event Abnormal Trading, Price Impact
- [[Cases/CASE-271|CASE-271 — Issuer Buyback While Possessing MNPI]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-285|CASE-285 — Research-Analyst Selective MNPI Disclosure]] — detectors: Cross-Product Economic Benefit, Price Impact, Volume Participation, Pre-Event Abnormal Trading
- [[Cases/CASE-427|CASE-427 — Insider Trading Before Earnings]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-428|CASE-428 — Insider Trading Before Dividend Action]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph, Benchmark-Window Participation, Price Impact
- [[Cases/CASE-429|CASE-429 — Insider Trading Before Buyback Announcement]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-430|CASE-430 — Insider Trading Before Capital Raise]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-431|CASE-431 — Insider Trading Before Bankruptcy or Restructuring]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-432|CASE-432 — Insider Trading Before Rating Action]] — detectors: Pre-Event Abnormal Trading, Related-Account Graph
- [[Cases/CASE-460|CASE-460 — Pump Before Insider Sale]] — detectors: Price Impact, Volume Participation, Rapid Position Reversal, Pre-Event Abnormal Trading

## Calibration

Use the per-case workspace as the starter rule. Replace fixed thresholds with liquidity/session/participant profiles after historical replay.
