---
project: Orleans-Rules-learning
type: architecture
tags: [orleans, architecture, tutorial]
---
# Tutorial Architecture

Conceptual components:

`Producer/API → Dispatcher → Orleans grain → grain state → facts → rules engine → result`

Supporting development tooling:

`Aspire → runs/observes services`
`Orleans Dashboard → visualizes Orleans activity`
`Storage provider → persists grain state`

Possible grains:
- `OrderBookGrain`
- `TraderGrain`
- `OrderGrain`
- `SurveillanceGrain`

Related:
- [[Tutorial Overview]]
- [[Simple Surveillance Scenario]]
- [[../04 Rules Engine/Rules Execution Flow]]
- [[../05 Observability/DotNET Aspire with Orleans]]
