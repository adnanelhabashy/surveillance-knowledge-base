---
id: IMPL-HOME
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Market Surveillance Implementation Architecture

> **Purpose:** Production-minded reference architecture for implementing the 540 trading-surveillance scenarios with .NET 10, Orleans 10, Kafka and Microsoft RulesEngine. AI is intentionally **not** part of the critical-path implementation in this pack; see [[Implementation-Architecture/19 - AI Future Overview|AI Future Overview]] for the future boundary.

## Recommended architecture in one sentence

**Kafka provides ordered/replayable market events; Orleans grains own keyed live state; reusable detectors turn state into facts; Microsoft RulesEngine evaluates only candidate rule packs; alerts go back to Kafka and are persisted asynchronously; a separate replay Orleans cluster validates rules without disturbing live surveillance.**

## The design goals

1. **Simple to reason about:** one owner for each piece of mutable state.
2. **Stable under load:** no global surveillance grain and no synchronous database write on the order-event hot path.
3. **Fast:** evaluate only rules relevant to the emitted fact, not all 540 cases for every event.
4. **Explainable:** every alert carries the rule version, detector version and evidence snapshot.
5. **Replayable:** Kafka/archive is the recovery and test source of truth for high-volume market events.
6. **Dynamic:** rules are versioned data, not hard-coded release-only logic.
7. **Expandable:** extra adapters activate the cases that require client orders, ownership, settlement, securities lending, routing or communications data.

## Start here

- [[Implementation-Architecture/01 - Recommended Architecture|1. Recommended Architecture]]
- [[Implementation-Architecture/02 - Container Architecture|2. Container Architecture]]
- [[Implementation-Architecture/03 - Orleans Silo Architecture|3. Orleans Silo Architecture]]
- [[Implementation-Architecture/04 - Grain Catalog|4. Grain Catalog]]
- [[Implementation-Architecture/06 - Event and Data Contracts|6. Event and Data Contracts]]
- [[Implementation-Architecture/07 - Detection and Rules Engine|7. Detection and Rules Engine]]
- [[Implementation-Architecture/Coverage/00 - 540 Case Coverage Strategy|540 Case Coverage Strategy]]
- [[Implementation-Architecture/Coverage/01 - 540 Case Coverage Matrix|540 Case Coverage Matrix]]
- [[Implementation-Architecture/12 - Deployment Options|Deployment Options]]
- [[Implementation-Architecture/13 - Capacity and Hardware Sizing|Capacity and Hardware Sizing]]
- [[Implementation-Architecture/20 - Implementation Roadmap|Implementation Roadmap]]
- [[Implementation-Architecture/22 - Architecture Options and Trade-offs|Architecture Options and Trade-offs]]
- [[Implementation-Architecture/Diagrams/00 - Diagram Index|Diagram Index]]
- [[Implementation-Architecture/Archetypes/00 - Archetype Index|Archetype Index]]

## Coverage model

The 540 case notes currently map to **22 implementation archetypes** and **22 reusable detector concepts**. The architecture uses those reusable primitives instead of attempting to build 540 independent streaming programs.

- Total case notes validated: **540**
- Implementation archetypes: **22**
- Unmapped case notes: **0**

## Important scope boundary

"Designed to cover all 540" means the platform has an implementation path for every catalog scenario. Some cases cannot be detected from exchange order/trade data alone. They become active only when the required structured input exists (for example beneficial ownership, client order instructions, borrow/settlement, securities lending, routing decisions, account-security data, or external communications/news signals).

## Existing knowledge links

- [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]]
- [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
