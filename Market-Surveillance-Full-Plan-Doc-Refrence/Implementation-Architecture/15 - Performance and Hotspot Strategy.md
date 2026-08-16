---
id: IMPL-15
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Performance and Hotspot Strategy

## The critical hot path

```text
Kafka event -> stream processor -> one state-owner grain -> local detectors -> candidate rule worker -> alert topic
```

Keep it short.

## Performance rules

- No synchronous blocking inside grains.
- Avoid remote calls in tight loops.
- Batch/fan-out only when semantically safe.
- Send compact immutable DTOs between grains.
- Do not copy a full order book into every rule evaluation.
- Keep evidence references/event ids on the hot path; build large forensic bundles asynchronously.
- Use fixed/bounded windows.
- Precompute liquidity/volatility baselines outside the event critical path.

## Hot book strategy

A single `OrderBookGrain` can become hot because one book is intentionally serialized. Fix it in this order:

1. Profile turn duration.
2. Remove logging/string formatting from per-event hot path.
3. Reduce allocations.
4. Keep only necessary depth/active-order state.
5. Offload detector/rule CPU to stateless local workers.
6. Move historical/evidence persistence off path.
7. Place hot books on high-clock silos if needed.
8. Only then consider a specialized non-Orleans order-book processor feeding grains; do not prematurely split one book's authoritative state across grains.

## Participant fan-out

An execution may touch buyer, seller, trader, account, owner and instrument. Do not synchronously wait on every global aggregate before continuing the book. Distinguish:

- **required-before-rule state** → await;
- **eventual aggregate** → publish/fan-out asynchronously with idempotency.

## Rule cache

Rule runtime objects are cached per active version. Never parse JSON/compile expressions per event.
