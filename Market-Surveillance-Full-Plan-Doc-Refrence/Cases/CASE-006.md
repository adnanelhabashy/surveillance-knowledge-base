---
id: CASE-006
type: surveillance-case
case_number: 6
title: "Circular Trading"
status: implementation-seeded
implementation_archetype: wash_matched
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 6. Circular Trading

## Description

Securities are passed through several related accounts and eventually return to the original beneficial owner to create artificial volume.

## Surveillance families

- [[Families/FAMILY-03|Wash, self, matched, prearranged & circular trading]]
- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]
- [[Detectors/DETECTOR-08|Circular Transaction Graph]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-351|Variable-Quantity Circular Trading]]
- [[Cases/CASE-122|Beneficial-Owner Evasion]]
- [[Cases/CASE-352|Cross-Venue Circular Trading]]
- [[Cases/CASE-007|Self-Trading]]
- [[Cases/CASE-508|Beneficial-Owner Cross-Broker Self-Trade]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial deterministic graph/cycle model
- **Detection mode:** Rules/graph algorithms; AI not required for the alert decision; ownership/reference data strongly improves detection
- **Rule logic (starter):** Flag executions that produce no meaningful change in beneficial ownership or show repeated highly synchronized counterparties, prices and quantities consistent with matched/prearranged/circular transfer rather than competitive trading.
- **Orleans grains/state:** TradeGrain, AccountGrain, InvestorGrain, RelationshipGrain, InstrumentGrain, SurveillanceGrain; maintain counterparty pairs, beneficial-owner mapping, trade cycles and rolling transferred quantity
- **Required event fields:** tradeId, eventTime, buyAccountId, sellAccountId, buyTraderId, sellTraderId, buyBeneficialOwnerId, sellBeneficialOwnerId, instrumentId, price, quantity, venueId, orderIds, tradeCondition
- **Time window(s):** Immediate same-trade check; 1–60 s matched-order window; rolling 30 min and trading-day counterparty/cycle windows
- **Thresholds/calibration:** Self/wash: same beneficial owner is a high-severity condition. Matched: start with price equal/within 1 tick, quantity similarity ≥ 90%, time difference ≤ 5 s and ≥ 3 repeated pairings in 30 min. Circular: cycle length 3–6 accounts within 30 min and quantity similarity ≥ 80%.
- **Alert evidence:** Matched buy/sell order IDs; counterparties and beneficial owners; timestamps; price/quantity similarity; repeated pair matrix; circular path/graph; percentage of participant and market volume
- **Implementation note:** Starter engineering model only. Identity resolution and as-of reference correctness are prerequisites for strong beneficial-owner claims. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Graph detection architecture

Circular Trading is fundamentally a directed-graph cycle problem.

Example:

```text
Investor A -> Investor B -> Investor C -> Investor A
```

Recommended detection pipeline:

1. Resolve Account -> Investor -> Beneficial Owner as-of source sequence.
2. Build a directed multigraph per instrument and rolling time window.
3. Enumerate bounded cycles, normally 3–6 nodes, using bounded DFS or Johnson-style cycle enumeration.
4. Validate the cycle using economic/conservation tests:
   - quantity similarity
   - notional similarity
   - round-trip timing
   - near-zero net ownership change
   - repetition
   - share of instrument/market volume
5. Store the exact cycle path and matched trades as alert evidence.

For wider rings, Label Propagation or Louvain may be used as supporting community analysis rather than replacing exact cycle evidence.

## AI / ML architecture alignment

AI should rank suspicious cycles rather than attempt to replace exact graph detection.

Recommended later supervised model after analyst labels exist:

- **Model family:** CPU GBDT such as LightGBM/XGBoost/ML.NET
- **Features:** cycle length, conservation ratio, timing tightness, repeated-cycle count, internal-volume ratio, market-volume share, owner-resolution confidence
- **Purpose:** investigation priority / risk ranking
- **Deployment:** versioned ONNX artifact scored in-process from .NET where practical
- **Unsupervised anomaly models:** review/triage only, especially for unknown group behavior

Identity resolution is more important than model choice for this case.

See [[Implementation-Architecture/AI-and-Deterministic-Detection-Decision-Architecture|AI and Deterministic Detection Decision Architecture]].

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
