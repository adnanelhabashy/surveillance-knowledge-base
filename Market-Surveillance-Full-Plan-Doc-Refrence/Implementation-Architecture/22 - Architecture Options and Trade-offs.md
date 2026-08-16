---
id: IMPL-22
type: architecture
status: reference
tags:
  - surveillance/implementation
---

# Architecture Options and Trade-offs

This note records the main alternatives so future design changes are deliberate rather than accidental.

## Silo topology options

### A. Homogeneous live silos — **recommended first**

Every silo hosts all grain types and stateless rule workers.

**Pros**

- simplest deployment and upgrade model;
- Orleans can place/rebalance freely;
- losing one node does not remove an entire functional capability;
- easy horizontal scale.

**Cons**

- CPU-heavy rule work and memory-heavy books share the same node pool.

**Decision:** use this first. Measure before adding role specialization.

### B. Role-specialized silos — advanced optimization

Examples: book silos, graph/coordination silos, rule-worker silos.

**Pros:** hardware can be tuned by workload; noisy workloads can be isolated.

**Cons:** placement/configuration complexity; more failure modes; uneven capacity can strand grain types.

**Use only when:** production metrics prove one workload class needs isolation.

### C. Separate Orleans cluster per surveillance family — **not recommended**

This fragments state and creates network/API integration between manipulation families that frequently need the same participant, position and market facts.

## Grain-model options

### A. Very fine-grained: OrderGrain for every order

Easy object mapping, but creates many activations and extra calls in the hottest path.

**Not recommended for the live baseline.**

### B. Very coarse-grained: one InstrumentSurveillanceGrain

Simple initially, but risks a "god grain" which owns the book, participants, positions, rules and alerts for one instrument.

**Not recommended.**

### C. Hybrid keyed state owners — **recommended**

- `OrderBookGrain` owns book/order lifecycle.
- `ParticipantInstrumentGrain` owns participant behavior windows.
- specialized grains own auctions, benchmarks, relationships, positions and external domains.
- stateless workers evaluate rules.

This keeps correctness boundaries clear while allowing independent scaling.

## Rule deployment options

### A. Embedded in silos — **recommended**

Lowest latency and no central network bottleneck.

### B. Dedicated rules microservice

Use only if organizational/governance isolation is more important than latency and operational simplicity.

### C. Hard-coded C# only

Fast, but weak for surveillance teams who need controlled threshold/rule changes without release cycles. Keep hard-coded detector math, not surveillance policy.

## Network options

### A. Flat Docker bridge

Good for a laptop only.

### B. Segmented Docker networks — **recommended for Compose/pilot**

Ingress / application / data / observability zones.

### C. Kubernetes NetworkPolicy — **recommended for Kubernetes production**

Apply the same logical zones using namespaces/services/policies and keep data services private.

## Persistence options

### PostgreSQL baseline — **recommended**

Use for Orleans clustering/reminders, rules, alerts, cases and durable administration.

### Redis

Useful as an optional high-speed provider/cache. Adds another stateful dependency; only introduce it for a measured reason.

### Persist every grain update

Not appropriate for the market hot path. High-volume order state is replayable from Kafka/archive.

## Live/replay options

### One cluster for both

Simple but dangerous: replay can starve the closing/opening auction.

### Separate clusters — **recommended production design**

Same code, different `ClusterId`, consumer groups and alert outputs.
