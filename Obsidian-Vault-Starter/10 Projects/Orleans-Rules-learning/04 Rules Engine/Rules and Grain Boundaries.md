---
project: Orleans-Rules-learning
type: design
tags: [orleans, rules-engine, architecture]
---
# Rules and Grain Boundaries

Keep responsibilities separate.

## Grain responsibility
- own entity state,
- enforce core invariants,
- provide facts/snapshots,
- coordinate actor-level behavior.

## Rules engine responsibility
- evaluate configurable business conditions,
- return rule outcomes,
- allow business logic to evolve independently from actor identity/state.

Avoid storing the entire rules engine implementation inside every grain.

Related:
- [[Dynamic Rules Engine]]
- [[Rules Execution Flow]]
- [[../02 Grain State/Persistence Design Rules]]
- [[../90 Decisions/Keep Rules Separate from Grain State]]
