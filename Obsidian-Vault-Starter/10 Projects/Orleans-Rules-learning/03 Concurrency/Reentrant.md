---
project: Orleans-Rules-learning
type: concept
tags: [orleans, concurrency, reentrant]
---
# Reentrant

`[Reentrant]` allows an activation to begin processing another request while an earlier request is waiting on an asynchronous operation.

Why it exists:
- to avoid certain actor-to-actor call cycles from blocking,
- to allow useful interleaving when the grain design can tolerate it.

Risk:
Two logical operations can now interleave around `await` points, so invariants must be designed carefully.

Use it when you understand the call chain and need reentrancy. Do not add it as a general performance switch.

Related:
- [[Orleans Concurrency Mental Model]]
- [[When to Use Concurrency Attributes]]
