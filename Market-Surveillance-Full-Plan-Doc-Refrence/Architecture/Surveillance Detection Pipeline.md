---
type: architecture-note
tags:
  - surveillance/architecture
---

# Surveillance Detection Pipeline

This note is a starting bridge from the case graph to the Orleans + dynamic-rules implementation.

```text
Market events / reference data
            ↓
         Orleans
            ↓
Reusable behavioral detectors
            ↓
     Dynamic rules engine
            ↓
          Alerts
            ↓
 Investigation / case workflow
```

## Graph entry points

- [[MOCs/01 - Surveillance Case Map|540-case surveillance map]]
- [[MOCs/03 - Reusable Detector Map|Reusable detector map]]
- [[MOCs/02 - SMARTS Public Coverage|SMARTS publicly described coverage]]

## Design principle

Do not build 540 isolated algorithms. Build reusable state and behavioral detectors, then combine their facts into many dynamic rules.
