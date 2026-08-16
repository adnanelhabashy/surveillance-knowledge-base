---
project: Orleans-Rules-learning
type: concept
tags: [orleans, concurrency, readonly]
---
# ReadOnly

`[ReadOnly]` communicates that a grain call does not modify grain state.

It can enable Orleans to schedule compatible read-only operations more flexibly.

A method should only be marked read-only when it truly does not change logical grain state.

Example:
`GetSnapshotAsync()` is a natural candidate when it only returns the current snapshot.

Related:
- [[Orleans Concurrency Mental Model]]
- [[When to Use Concurrency Attributes]]
