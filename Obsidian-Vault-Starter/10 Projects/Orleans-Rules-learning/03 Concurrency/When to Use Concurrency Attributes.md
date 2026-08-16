---
project: Orleans-Rules-learning
type: guide
tags: [orleans, concurrency, decision]
---
# When to Use Concurrency Attributes

## Default choice
Use Orleans' default serialized grain execution.

## Consider `ReadOnly`
When the operation only reads state and has no logical side effects.

## Consider `Reentrant`
When an async grain-to-grain call flow can re-enter the activation and the state machine is safe under interleaving.

## Consider `AlwaysInterleave`
When one specific method must remain interleavable and is safe regardless of the other in-flight request.

## Consider `MayInterleave`
When eligibility depends on the incoming request itself.

## Warning sign
If you are adding a concurrency attribute merely because "more parallel must be faster," stop and model the grain boundary first.

Related:
- [[Orleans Concurrency Mental Model]]
- [[../02 Grain State/State Lifecycle]]
