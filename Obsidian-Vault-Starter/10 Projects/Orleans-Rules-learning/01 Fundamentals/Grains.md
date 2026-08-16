---
project: Orleans-Rules-learning
type: concept
tags: [orleans, grain]
---
# Grains

A grain is the primary Orleans programming abstraction.

A grain combines:
- identity
- behavior
- optionally persistent state
- Orleans-managed activation and placement
- serialized request processing by default

Examples from our surveillance-style learning model:
- `TraderGrain(traderId)`
- `OrderGrain(orderId)`
- `OrderBookGrain(orderBookId)`
- `SurveillanceGrain(...)`

The important idea is that callers address a grain by identity rather than by finding a server instance themselves.

Related:
- [[Orleans Mental Model]]
- [[Grain Activation and Location]]
- [[../02 Grain State/State Lifecycle]]
