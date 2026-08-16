---
project: Orleans-Rules-learning
type: concept
tags: [rules-engine, dynamic-rules]
---
# Dynamic Rules Engine

The learning goal is to combine Orleans with a rules engine whose business rules can change without rewriting the grain model.

The grain should own domain state and behavior.
The rules engine should evaluate business conditions using explicit facts.

Conceptual flow:

`Event → grain updates/derives facts → rules evaluate facts → result/action → grain/system handles result`

Related:
- [[Rules and Grain Boundaries]]
- [[Rules Execution Flow]]
- [[../06 Tutorial/Simple Surveillance Scenario]]
