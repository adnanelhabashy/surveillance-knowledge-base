---
project: Orleans-Rules-learning
type: project-hub
status: active
tags:
  - moc
  - orleans
  - rules-engine
---
# Orleans-Rules-learning

## Goal
Learn Microsoft Orleans deeply enough to design and build a practical .NET 10 system using grains, persistence, concurrency, observability, and a dynamic rules engine.

## Current learning map
### Orleans fundamentals
- [[01 Fundamentals/Orleans Mental Model]]
- [[01 Fundamentals/Grains]]
- [[01 Fundamentals/Silos Clusters and Clients]]
- [[01 Fundamentals/Grain Activation and Location]]

### Grain state
- [[02 Grain State/Grain State Persistence]]
- [[02 Grain State/State Lifecycle]]
- [[02 Grain State/Persistence Design Rules]]

### Concurrency
- [[03 Concurrency/Orleans Concurrency Mental Model]]
- [[03 Concurrency/Reentrant]]
- [[03 Concurrency/AlwaysInterleave]]
- [[03 Concurrency/MayInterleave]]
- [[03 Concurrency/ReadOnly]]
- [[03 Concurrency/When to Use Concurrency Attributes]]

### Rules engine
- [[04 Rules Engine/Dynamic Rules Engine]]
- [[04 Rules Engine/Rules and Grain Boundaries]]
- [[04 Rules Engine/Rules Execution Flow]]

### Observability and development tooling
- [[05 Observability/Orleans Dashboard]]
- [[05 Observability/DotNET Aspire with Orleans]]
- [[05 Observability/Aspire vs Production Observability]]

### Hands-on tutorial
- [[06 Tutorial/Tutorial Overview]]
- [[06 Tutorial/Simple Surveillance Scenario]]
- [[06 Tutorial/Tutorial Architecture]]
- [[06 Tutorial/Next Learning Topics]]

## Key relationships
Orleans gives us the **stateful actor model**. Grain state persistence gives those actors durable memory. Orleans concurrency rules control how calls can overlap. The dynamic rules engine evaluates business behavior using facts derived from grain state. Aspire/dashboard tooling lets us see the system while learning and operating it.

## Decisions
- [[90 Decisions/Use Project-Scoped Notes First]]
- [[90 Decisions/Keep Rules Separate from Grain State]]

## Reference
- [[99 Reference/Glossary]]
