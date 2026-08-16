---
project: Orleans-Rules-learning
type: concept
tags: [orleans, state]
---
# State Lifecycle

Think of grain state in two forms:

- **Working state** — the in-memory state of the current activation.
- **Durable state** — the last state successfully stored in the persistence provider.

A state change is not durable merely because a property changed in memory.

Conceptual flow:

`Load → process message → mutate state → persist → continue`

Related:
- [[Grain State Persistence]]
- [[Persistence Design Rules]]
- [[../03 Concurrency/Orleans Concurrency Mental Model]]
