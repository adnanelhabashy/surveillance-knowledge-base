---
project: Orleans-Rules-learning
type: reference
tags: [glossary, orleans]
---
# Glossary

**Activation** — the in-memory runtime instance representing a grain at a point in time.

**Client** — code that calls Orleans grains without normally hosting them.

**Cluster** — cooperating Orleans silos.

**Grain** — Orleans virtual actor abstraction with identity and behavior.

**Grain state** — data associated with a grain, optionally persisted.

**Interleaving** — allowing request execution to overlap logically around asynchronous waits.

**Reentrancy** — allowing another request to enter an activation while a previous request is incomplete.

**Rules engine** — evaluates configurable business conditions using supplied facts.

**Silo** — a process hosting Orleans grain activations.

**Virtual actor** — actor abstraction whose lifecycle and location are managed by the runtime.
