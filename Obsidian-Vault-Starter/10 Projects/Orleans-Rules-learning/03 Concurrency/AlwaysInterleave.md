---
project: Orleans-Rules-learning
type: concept
tags: [orleans, concurrency, interleave]
---
# AlwaysInterleave

`[AlwaysInterleave]` marks a grain interface method so calls to that method are always eligible to interleave with other requests.

Simple use case:
A safe informational or coordination call must remain callable while another longer operation is waiting.

Because it weakens the default serialized execution model, use it only for methods whose behavior remains safe under interleaving.

Related:
- [[Orleans Concurrency Mental Model]]
- [[MayInterleave]]
- [[ReadOnly]]
