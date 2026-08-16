---
project: Orleans-Rules-learning
type: decision
status: accepted
tags: [decision, rules-engine, orleans]
---
# Keep Rules Separate from Grain State

## Decision
Model persistent domain state in grains and configurable business conditions in the rules layer.

## Why
This keeps actor identity/state stable while allowing business rules to change more frequently.

Related:
- [[../04 Rules Engine/Rules and Grain Boundaries]]
- [[../02 Grain State/Persistence Design Rules]]
