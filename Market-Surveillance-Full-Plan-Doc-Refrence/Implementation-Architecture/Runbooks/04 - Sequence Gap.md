---
id: RUNBOOK-04
type: runbook
status: reference
tags:
  - surveillance/implementation
---


# Runbook — Market Sequence Gap

1. Mark affected venue/instrument or feed partition degraded.
2. Record first missing sequence and last valid state.
3. Halt or tag book-sensitive alert decisions as incomplete.
4. Obtain retransmission/snapshot according to feed protocol.
5. Rebuild and validate book checksum/reference state.
6. Resume normal processing.
7. Replay gap interval and reconcile alerts.
