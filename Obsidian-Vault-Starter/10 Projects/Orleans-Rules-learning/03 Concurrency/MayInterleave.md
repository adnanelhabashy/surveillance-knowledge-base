---
project: Orleans-Rules-learning
type: concept
tags: [orleans, concurrency, interleave]
---
# MayInterleave

`[MayInterleave]` lets a grain decide dynamically whether an incoming request may interleave.

Think of it as conditional concurrency.

Example reasoning:
- requests touching independent logical partitions may be allowed to overlap,
- requests that could mutate the same invariant remain serialized.

This is more advanced than the default model and should be introduced only when the concurrency rule is clear and testable.

Related:
- [[AlwaysInterleave]]
- [[Orleans Concurrency Mental Model]]
- [[When to Use Concurrency Attributes]]
