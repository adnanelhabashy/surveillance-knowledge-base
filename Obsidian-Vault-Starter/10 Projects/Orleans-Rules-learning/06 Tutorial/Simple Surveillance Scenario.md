---
project: Orleans-Rules-learning
type: scenario
tags: [surveillance, tutorial, orleans]
---
# Simple Surveillance Scenario

A small market-surveillance scenario gives us realistic identities and state without making the tutorial too large.

Example entities:
- Trader
- Order
- Order book
- Market event
- Surveillance result

Example grain methods we discussed:
- `ProcessAsync(...)`
- `GetSnapshotAsync()`
- `ResetAsync()`

This scenario is simple enough for learning but close enough to the larger surveillance architecture to remain useful.

Related:
- [[Tutorial Architecture]]
- [[../04 Rules Engine/Rules Execution Flow]]
- [[../01 Fundamentals/Grains]]
