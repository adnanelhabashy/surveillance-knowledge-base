# Architecture Test Matrix

| Test | Expected result |
|---|---|
| Replay same canonical event twice | state changes once by `EventId` |
| Crash after raw consume before release | raw event is replayed after restart |
| Crash after canonical publish before source commit | duplicate may replay; canonical/state not lost |
| Missing sequence above safe watermark | assembler waits |
| Missing sequence at/below proven safe watermark | `CoverageGapEvent` emitted |
| Redis unavailable | no new unproven gap; unresolved hole blocks release |
| Raw header malformed | durable data-quality event; canonical state not polluted |
| Header/payload identity mismatch | durable data-quality event; record not canonicalized |
| Same sequence + same identity | replay duplicate dropped/idempotent |
| Same sequence + distinct identities | durable `SourceSequenceConflictEvent` |
| Missing Required topic | coverage degraded |
| Missing Optional/NotProvisioned topic | no automatic false degradation |
| Investor then Account then Order | Silo resolves Account → Investor |
| Order arrives before needed reference | Silo follows explicit unresolved-reference policy; Ingestor does not enrich it |
| Canonical sequence integration run | monotonically increasing sequence per domain |
