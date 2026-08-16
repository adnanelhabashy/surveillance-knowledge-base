---
project: Orleans-Rules-learning
type: concept
tags: [orleans, fundamentals]
---
# Orleans Mental Model

Microsoft Orleans lets you model application entities as **virtual actors** called [[Grains]].

Instead of manually managing object lifetime, placement, locking, and remote communication, Orleans handles activation and routing.

A useful mental model:

`Request/Event → Grain identity → Orleans runtime → Grain activation → state + behavior`

Related:
- [[Silos Clusters and Clients]]
- [[Grain Activation and Location]]
- [[../03 Concurrency/Orleans Concurrency Mental Model]]
- [[../02 Grain State/Grain State Persistence]]
