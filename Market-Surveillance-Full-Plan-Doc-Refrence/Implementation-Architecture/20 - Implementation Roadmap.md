---
id: IMPL-20
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Implementation Roadmap

## Phase 0 — contracts and replay harness

- canonical event envelope
- OrderEvent / ExecutionEvent / MarketStateEvent
- Kafka topics
- deterministic session replay tool
- event ids/sequence-gap handling

**Exit:** can replay a session byte-for-byte into canonical events.

## Phase 1 — Orleans market-state core

- 3-silo-capable host
- OrderBookGrain
- InstrumentGrain
- ParticipantInstrumentGrain
- Account/Trader/Owner reference grains
- metrics/tracing

**Exit:** deterministic book/participant state survives process failures via replay.

## Phase 2 — first reusable detectors

Start with the 22 detector primitives already linked in the vault:

- cancellation ratio
- order lifetime
- displayed-size anomaly
- multi-level depth pressure
- opposite-side execution
- ownership relation
- price/time/quantity matching
- circular graph
- price impact
- participation
- auction impact
- benchmark participation
- cross-product benefit
- pre-event abnormal trading
- short/borrow/settlement status
- related-account graph
- trade-report accuracy/timing
- cross-venue sync
- position concentration
- liquidity concentration
- message burst rate
- rapid position reversal

## Phase 3 — dynamic rules platform

- PostgreSQL rule store
- Microsoft RulesEngine integration
- candidate routing
- stateless worker evaluation
- versioning/rollback/shadow mode

**Exit:** rule changes need no surveillance-binary release.

## Phase 4 — first production case families

Recommended order:

1. spoofing/layering
2. wash/self/matched
3. marking close/open and auction
4. ramping/momentum/price-volume
5. order stuffing/probing
6. related-account coordination

These exercise the architecture before adding external domains.

## Phase 5 — extended structured data

Add in priority order based on available enterprise feeds:

- positions/holdings/beneficial ownership
- corporate events/insider relationships
- client order data
- short/borrow/settlement
- trade reporting
- securities lending
- routing/venue economics
- account security

## Phase 6 — full 540 validation

Use [[Implementation-Architecture/Coverage/01 - 540 Case Coverage Matrix|the coverage matrix]] to move every case from Designed → Implemented → Calibrated → Production.

## Phase 7 — AI

Add AI as a governed enrichment/detection layer after the deterministic platform is stable. See [[Implementation-Architecture/19 - AI Future Overview|AI Future Overview]].
