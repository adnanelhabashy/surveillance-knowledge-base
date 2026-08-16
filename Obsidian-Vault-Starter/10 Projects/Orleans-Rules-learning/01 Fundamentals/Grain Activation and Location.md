---
project: Orleans-Rules-learning
type: concept
tags: [orleans, activation]
---
# Grain Activation and Location

When code asks Orleans for a grain reference, it is asking for the logical actor identity.

Orleans decides:
1. whether an activation already exists,
2. where it is running,
3. whether a new activation is needed,
4. how to route the call.

This is why application code should avoid thinking in terms of "which server owns this object?"

Related:
- [[Grains]]
- [[Silos Clusters and Clients]]
- [[../03 Concurrency/Orleans Concurrency Mental Model]]
