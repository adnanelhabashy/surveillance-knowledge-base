---
project: Orleans-Rules-learning
type: guide
tags: [orleans, persistence, design]
---
# Persistence Design Rules

Use persistent grain state for information that logically belongs to the grain and must survive activation loss.

Avoid treating grain storage as:
- an event archive,
- a replacement for every analytical database,
- a place for huge unbounded histories.

Prefer compact state representing the current durable truth needed by the grain.

For surveillance/event systems, durable state may contain current counters, last processed identifiers, compact rolling facts, or configuration references, while raw events can live in Kafka/archive storage.

Related:
- [[Grain State Persistence]]
- [[../04 Rules Engine/Rules and Grain Boundaries]]
