---
type: drop-service
status: current
tags:
  - drop/service
  - drop/trade-enrichment
---

# Trade Enrichment Service

Consumes `mme.drop.parsed.trades`, pairs opposite trade sides via Redis and publishes `mme.drop.enriched.trades` as matched/enriched trade pairs.

## State

`pending:trade:{lifecycle}:{buy|sell}` Redis lists hold unmatched trade sides.

## Current behavior important to data consumers

- Manual Kafka offset commit per batch.
- Separate Redis/Kafka steps mean pending-state update, output publish and input offset commit are not one atomic transaction.
- Crash windows can duplicate pending entries or enriched output.
- Deterministic source-side evidence such as `matchId`, order IDs, Kafka position and MME sequence should be preserved by downstream consumers.
