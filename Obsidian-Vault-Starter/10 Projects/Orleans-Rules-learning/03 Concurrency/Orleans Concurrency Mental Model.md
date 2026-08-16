---
project: Orleans-Rules-learning
type: concept
tags: [orleans, concurrency]
---
# Orleans Concurrency Mental Model

By default, an Orleans grain activation processes one request at a time.

That default is powerful because grain code can often reason about state without ordinary multithreaded locking.

Concurrency attributes deliberately relax or annotate that behavior.

Important tools:
- [[Reentrant]]
- [[AlwaysInterleave]]
- [[MayInterleave]]
- [[ReadOnly]]
- [[When to Use Concurrency Attributes]]

Start from the default. Add concurrency features only when a real call-flow or throughput requirement justifies them.
