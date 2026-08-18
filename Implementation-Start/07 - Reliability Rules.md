# Ingestion Reliability Rules

## Non-negotiable rules

1. Prefer duplicates over silent loss.
2. Use deterministic `EventId` for replay idempotency.
3. Do not acknowledge source data while its only copy is in a volatile reorder buffer.
4. Do not declare a sequence gap until the safe watermark proves the sequence is absent.
5. Redis failure must never cause invented gaps.
6. Canonical ordering must be preserved from assembler to Silo state application.
7. Corrupt/lying records are quarantined with forensic evidence and do not enter canonical state.
8. Sequence identity conflicts are durable data-quality events, not only logs.
9. Missing required source topics degrade coverage; missing optional/not-provisioned topics do not automatically do so.
10. The Silo may replay canonical events without changing final state.

## Commit rule

Incorrect:

```text
consume → buffer in RAM → commit source offset → publish later
```

Target:

```text
consume → validate/buffer → safely release/publish → advance safe source offset
```

If a crash happens before the safe source offset advances, Kafka replays the input and deterministic identity removes duplicate effects.

## Redis/watermark rule

The watermark is proof that absent sequence numbers are truly missing; it is not required to recognize already-contiguous buffered sequences.

When watermark proof disappears:

```text
contiguous known data → may continue
unresolved missing sequence → stop there
```

Never skip the unresolved number based on a guess.
