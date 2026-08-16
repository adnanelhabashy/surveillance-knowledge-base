---
project: Orleans-Rules-learning
type: flow
tags: [rules-engine, flow]
---
# Rules Execution Flow

Example surveillance flow:

1. Dispatcher receives a market event.
2. It calls the relevant `OrderBookGrain`.
3. The grain deduplicates using the market event identity.
4. Grain state is updated.
5. Facts/snapshot are built.
6. Related `TraderGrain` state can be queried when required.
7. Facts are sent to the rules engine.
8. Rule results produce alerts, classifications, or follow-up actions.
9. Durable grain state is persisted when required.

Related:
- [[Dynamic Rules Engine]]
- [[Rules and Grain Boundaries]]
- [[../06 Tutorial/Tutorial Architecture]]
