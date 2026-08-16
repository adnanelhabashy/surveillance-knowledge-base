---
id: IMPL-13
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Capacity and Hardware Sizing

> These are **starting engineering sizes**, not guaranteed capacity numbers. Final sizing must come from full-session replay at 2–3× the expected peak event rate with all enabled rule packs.

## Sizing principles

The main drivers are:

1. Peak order/message events per second, not daily average.
2. Number of simultaneously active `venue|instrument` books.
3. Number of active participant/instrument windows.
4. Depth retained per book.
5. Number of candidate rule packs per fact.
6. Relationship/coordination graph density.
7. Alert/evidence volume.
8. Replay speed target.

## Suggested starting tiers

| Tier | Live silos | Per silo | Ingest/stream processors | Typical use |
|---|---:|---|---|---|
| Dev | 1 | 4 vCPU / 8–12 GB | 1 small | developer machine |
| Pilot | 3 | 8 vCPU / 16–32 GB | 2 × 4 vCPU / 8 GB | realistic UAT / modest market |
| Medium production | 5 | 16 vCPU / 32–64 GB | 3+ × 4–8 vCPU / 8–16 GB | sustained multi-feed surveillance |
| Large production | 7+ | 16–32 vCPU / 64 GB+ | scale by Kafka partitions | only after replay benchmark |

## Kafka starting point

Production baseline:

- 3 brokers
- 8+ vCPU / 16–32 GB RAM per broker as a starting point
- fast SSD/NVMe
- replication factor 3
- storage sized from peak byte rate × retention + replication + safety margin

## PostgreSQL starting point

- 8 vCPU / 16–32 GB for pilot/medium metadata and alert workload
- fast SSD
- HA standby
- separate connection pools for Orleans system tables vs application query/write workload if contention appears

Do not write raw tick/order history into PostgreSQL synchronously.

## Silo memory budget

Target normal operation below roughly **60–70% container memory** so the runtime has room for bursts, GC and activation movement. Alert before sustained 75–80%.

## Headroom target

A production live cluster should survive loss of one silo without exceeding safe CPU/memory. If 3 silos normally run at 75% CPU, you do not really have N+1 capacity.

## Benchmark acceptance test

Replay a worst historical session at:

- 1× real time
- 2× real time
- 3× real time

Measure:

- p50/p95/p99 event-to-fact latency
- p95/p99 alert latency
- Kafka lag
- silo CPU
- GC pause/time-in-GC
- activation counts
- grain call latency
- hot grain turn time
- alert writer lag

A good first SLO target for core real-time market-abuse patterns is sub-second alert generation under normal load, with tighter internal event-processing targets. Calibrate exact SLOs to regulatory/business requirements.
