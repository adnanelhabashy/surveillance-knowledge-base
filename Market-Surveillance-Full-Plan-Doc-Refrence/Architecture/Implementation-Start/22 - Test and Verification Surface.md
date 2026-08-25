---
id: IMPL-START-22
type: implementation-reference
status: code-verified-test-inventory
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - testing
  - verification
---

# Test and Verification Surface

Parent: [[16 - Development Implementation Snapshot]]

> [!NOTE]
> This note records **tests present in source** at the audited commit. It does not claim that every test was executed or passing during this vault audit.

## Test projects present

| Project | Verified source coverage examples |
|---|---|
| `TheEye.UnitTests` | DROP adapters/event IDs, feature fixtures, Galaxy projection/delta/reconnect/load and other cross-cutting unit tests |
| `TheEye.IngestionTests` | canonical envelope deserialization, DROP record headers/context, enriched trade mapping, ingestion settings, Kafka barrier coordination, keyed dispatch, reference identity and session calendar |
| `TheEye.SourceAssemblyTests` | canonical Kafka producer, collector/assembler, watermark reader, source offset safety, sequence buffer and topic sequence guards |
| `TheEye.SiloUnitTests` | actor scoping/keys, alert persistence/revision, bounded histories, circular graph/window/deep-scan, replay and wider grain/detector behavior |
| `TheEye.FeatureWriterTests` | CSV durability/recovery, archive worker, commit observer, PostgreSQL backend and PostgreSQL export |
| `TheEye.PersistenceIntegrationTests` | PostgreSQL/Orleans persistence, restart behavior, backup/restore, reminder persistence and persistence failure behavior |
| `TheEye.SyntheticDataTests` | synthetic data generator behavior |

Source roots:
- [TheEye.UnitTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.UnitTests)
- [TheEye.IngestionTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.IngestionTests)
- [TheEye.SourceAssemblyTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SourceAssemblyTests)
- [TheEye.SiloUnitTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SiloUnitTests)
- [TheEye.FeatureWriterTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.FeatureWriterTests)
- [TheEye.PersistenceIntegrationTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.PersistenceIntegrationTests)
- [TheEye.SyntheticDataTests](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SyntheticDataTests)

## Particularly important existing tests

### Ordering/source safety

The source-assembly and ingestion projects contain tests around:

- `KafkaBarrierCoordinator`;
- `SourceOffsetSafetyTracker`;
- `SourceSequenceBuffer`;
- `TopicSequenceGuard` and its state;
- `IngestorWatermarkReader`;
- `DropSourceAssembler`.

These are important because source ordering is a core surveillance correctness property.

### Circular detection

`TheEye.SiloUnitTests` contains dedicated circular graph and deep-scan tests, including `CircularGraphDetectorTests`, `CircularGraphWindowTests` and `CircularDeepScanRecallTests`, plus a circular replay harness.

### Persistence/recovery

`TheEye.PersistenceIntegrationTests` contains `GrainRestartTests`, `BackupRestoreTests`, `PersistenceFailureTests` and `DeepScanReminderTests`, demonstrating that recovery/failure behavior has explicit integration coverage.

### Feature archive durability

`TheEye.FeatureWriterTests` contains large dedicated suites for CSV partial recovery, archive worker behavior and PostgreSQL archive/export behavior.

## Missing acceptance proof for the known ordering defect

The repository's own current issue analysis still calls for stronger end-to-end acceptance evidence:

```text
authoritative single-stream replay
adversarial late-event fixtures
permutation-invariant rolling state/features
paced vs zero-speed comparison
PostgreSQL vs CSV parity
all seven feature datasets enabled
zero feature conflicts across all seven datasets
```

These should be treated as **required acceptance criteria for the ordering fix**, not as already-proven guarantees.

Source: [current_issue.md](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/current_issue.md).

Related: [[21 - Current Implementation Gaps and Known Defects]].
