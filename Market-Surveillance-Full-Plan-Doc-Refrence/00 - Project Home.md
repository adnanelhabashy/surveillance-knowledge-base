---
type: project-home
project: Market Surveillance
status: active
tags:
  - project/market-surveillance
---
a
# Market Surveillance — Project Home

This Obsidian project starts with **540 stock-market trading-surveillance scenarios** and turns them into a connected graph for the Orleans + dynamic-rules surveillance project.

## Start here

1. [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]] — browse the 540 cases by family.
2. [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]] — see the reusable facts that can power many cases.
3. [[Architecture/Surveillance Detection Pipeline|Surveillance Detection Pipeline]] — bridge the knowledge graph to the application architecture.
4. [[MOCs/02 - SMARTS Public Coverage|SMARTS Publicly Described Coverage]] — keep public SMARTS claims separate from our broader requirements catalog.

## Graph structure

```text
Project Home
    ├── Surveillance Families
    │       └── 540 Case Notes
    │               ├── Related Cases
    │               └── Reusable Detectors
    ├── SMARTS Public Coverage
    └── Architecture
```

## Current seed

- **Cases:** 540
- **Surveillance families:** 25
- **Reusable detector concepts:** 22
- **SMARTS mappings explicitly described in public source material:** 12 exact catalog entries
- **Additional catalog variants of publicly described behavior:** 55

## Next graph layers to add

- Orleans grains (`OrderBookGrain`, `TraderGrain`, `InvestorGrain`, `AccountGrain`, etc.)
- Market event types
- Facts produced by each grain
- Dynamic rule definitions
- Required fields/data sources
- Alert evidence and severity
- Test scenarios and replay datasets


## Implementation architecture

- [[Implementation-Architecture/00 - Implementation Architecture Home|Implementation Architecture]]
