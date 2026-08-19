---
type: redis-state-model
status: current
tags:
  - drop/redis
  - drop/current
---

# Redis State and Reference Cache

## Current key ownership

| Key pattern | Type | Writer | Purpose / readers |
|---|---|---|---|
| `mme.drop.ingestor:{instance}:next_mme_sequence_number` | String | Ingestor / Airflow reset | Next EGX replay checkpoint |
| `mme.drop.ingestor:{instance}:health` | Hash + TTL | Ingestor | Airflow runtime-health monitoring |
| `asset:{id}` | Hash | ReferenceDataCacheService | Asset lookups by enrichers / THE EYE reference resolution |
| `orderbook:{id}` | Hash | ReferenceDataCacheService | Order-book lookups by enrichers / THE EYE reference resolution |
| `participant:{id}` | Hash | ReferenceDataCacheService | Participant name/context |
| `actor:{id}` | Hash | ReferenceDataCacheService | Actor name/context |
| `account:{id}` and secondary `account:{name}` | Hash | ReferenceDataCacheService | Account lookup |
| `accountgroup:{id}` | Hash | ReferenceDataCacheService | Account-group reference |
| `accounttype:{id}` | Hash | ReferenceDataCacheService | Account-type reference |
| `investor:{id}` | Hash | ReferenceDataCacheService | Investor reference |
| `custodian:{id}` and secondary `custodian:{name}` | Hash | ReferenceDataCacheService | Custodian lookup |
| `corporateaction:{id}` | Hash | ReferenceDataCacheService | Corporate-action reference |
| `exchangerate:{currency}` | Hash | ReferenceDataCacheService | Exchange-rate reference |
| `system:initialbusinessdate` | Hash | ReferenceDataCacheService | Initial business date |
| `accountpos:{asset}:{participant}:{account}:{investor}` | Hash | ReferenceDataCacheService | Position updates |
| `endofreference:{yyyyMMdd}` | String | ReferenceDataCacheService | Reference-cache readiness gate for Airflow |
| `pending:trade:{lifecycle}:{buy|sell}` | List | TradeEnrichmentService | Unmatched trade-side matching state |

## Current reference warm-up

```mermaid
flowchart LR
    REF[Kafka reference topics] --> CACHE[ReferenceDataCacheService]
    CACHE --> R[(Redis hashes)]
    EORD[EndOfReferenceData] --> CACHE
    CACHE --> MARK[endofreference:date]
    MARK --> AF[Airflow readiness gate]
    AF --> ENR[Start order + trade enrichers]
```

Reference updates can occur after initial EORD; the cache continues consuming them.

## THE EYE current runtime usage

The active THE EYE ingestion profile deliberately avoids re-consuming pure reference/identity Kafka topics in its hot path.

Instead:

```text
existing ReferenceDataCacheService
        ↓
current Redis reference hashes
        ↓
THE EYE reference resolver / local read-through cache
        ↓
resolved order/trade context
```

This applies to current identity/instrument lookups such as participant, actor, account, investor, custodian, asset and order book.

The real-time ingestion worker still consumes live market-context messages that affect surveillance state directly, such as session changes, price limits, reference-price changes, BBO, equilibrium prices and circuit-breaker information.

See [[Architecture/Implementation-Start/10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]] and [[Architecture/Implementation-Start/16 - Trading-Only Acquisition and Topic Sequence Guard|Trading-Only Acquisition and Topic Sequence Guard]].

> [!CAUTION]
> Redis stores mutable current state. It is appropriate for the live lookup path, but a separate versioned snapshot/archive is required if historical replay must prove the exact reference value that existed at an old source sequence.
