---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Determinism and Scale

## Determinism — the guarantees

> Same `seed` + `runId` + config → **byte-identical** `events.jsonl`,
> `labels.jsonl`, `manifest.json`. Verified by a test that generates twice and
> compares bytes.

How it is enforced (all in code, not convention):

1. **Owned RNG algorithm** — `DeterministicRandom` implements SplitMix64
   directly. `System.Random`'s algorithm can change between .NET versions;
   owning the sequence means a dataset regenerated on a future runtime is still
   identical.
2. **Named sub-streams** — the root RNG is forked per consumer
   (`Fork("normal:101")`, `Fork("scenario:circular-trading")`,
   `Fork("positive:7")`). A fork's stream depends only on the root seed and its
   name, so adding a scenario or reordering consumers **cannot** shift anyone
   else's random draws.
3. **Content-derived identities** — event ids are `syn-{runToken}-{identity}`
   where identity encodes the thing itself (`o{orderId}-n`, `b{book}-q{index}`)
   and `runToken` derives from `(seed, runId)`. No GUIDs anywhere.
4. **Explicit ordering everywhere** — output order is a total order
   (event time → sort priority → stable sequence); nothing ever depends on hash
   enumeration order.
5. **Config snapshot** — `config.json` is written next to the data, so
   "which knobs produced this" is never guesswork.

Practical consequence: two machines, two months apart, same inputs → identical
datasets. Diff-friendly: **any** byte difference means the generator code
changed.

## Scale — measured, not theoretical

Measured on the dev machine (Apple M4, 16 GB RAM):

| | Training profile | 10× profile |
|---|---|---|
| Events | 152,593 | 1,525,886 |
| Matched trades | 19,807 | 197,994 |
| Labeled episodes | 600 | 6,000 |
| Wall time | ~1.6 s | 7.6 s |
| Peak RAM | ~160 MB | 1.6 GB |
| `events.jsonl` | ~220 MB | 2.2 GB |

Unit costs: **~1.05 KB RAM and ~1.5 KB disk per event**, **~200k events/second**.

### What limits a single run

- **No config caps** — no knob has an upper bound (validation only checks
  sanity, like `participantCount ≥ 3`).
- **RAM is the ceiling**: the generator materializes *all* wire events in
  memory before writing. On 16 GB that's ≈ **10M events per run**; beyond it,
  GC pressure dominates.
- **Time and disk are never the problem** at training scale (10M events ≈ 50 s,
  ≈ 15 GB JSONL).

### Scaling beyond one run

Unlimited by design: **vary `seed` (and `runId`)** per run and union the
datasets. 100 runs of the training profile = 60,000 labeled episodes / 15M
events, each run bounded in RAM. Distinct `runToken`s also mean their event ids
never collide, so multiple datasets can be replayed into the same silo.

## Replay, dedup, and the 1,024-event window

The silo's grains dedup incoming commands by lineage event id over a bounded
window of the **most recent 1,024 ids per book** (in-memory state today).

- **Forward consumption** of any size: fine — every event is new; old ids age
  out because nothing needs them again.
- **Kafka redelivery** (consumer restart, rebalance): always covers *recent*
  records — well inside the window → full no-op. This is what the window is
  sized for.
- **Replaying a large dataset into a live silo**: events older than the window
  have evicted ids → they look new → state **double-applies**. Not a generator
  bug; a consequence of non-persistent grain state. Clean re-runs: restart the
  silo, or generate with a fresh `runId` (new `runToken` → new identities).

The durable fix is grain persistence + a persisted dedup checkpoint/high-water
mark — already on the implementation roadmap, not a synthetic-data concern.
