---
project: Orleans-Rules-learning
type: concept
tags: [orleans, persistence, state]
---
# Grain State Persistence

Persistence separates a grain's **logical identity** from the lifetime of one in-memory activation.

Typical lifecycle:
1. Grain activates.
2. Orleans loads persisted state.
3. Grain processes calls and updates in-memory state.
4. The grain writes state using the configured storage provider.
5. Activation can disappear later.
6. A future activation reloads the stored state.

Persistence is what allows an Orleans entity to survive silo restarts and deactivation.

Related:
- [[State Lifecycle]]
- [[Persistence Design Rules]]
- [[../01 Fundamentals/Grain Activation and Location]]
