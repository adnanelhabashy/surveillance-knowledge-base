---
id: IMPL-START-19
type: implementation-reference
status: code-verified
audited_commit: 0b4af2e99e530ce56a94d894865c761b7d7306e8
tags:
  - surveillance/implementation
  - ml/feature-store
  - kafka
  - postgresql
---

# Feature Store and Archive Implementation

Parent: [[16 - Development Implementation Snapshot]]

## Runtime split

The feature path is physically split into two responsibilities:

```text
case evaluation / feature emission
        |
        v
TheEye.FeatureStore contracts + publisher support
        |
        v
Kafka feature topic
        |
        v
TheEye.FeatureWriter
        |
        +--> CSV archive
        +--> PostgreSQL feature_archive
```

## `TheEye.FeatureStore`

**Implemented.** The project contains the feature envelope/row model and supporting validation/revision logic, including `FeatureEnvelopeV1`, JSON/validation support, feature rows/metadata, evaluation revisions, revision comparison, publisher metrics and disabled/no-op publication.

Source: [TheEye.FeatureStore](https://github.com/adnanelhabashy/the-eye-v2/tree/0b4af2e99e530ce56a94d894865c761b7d7306e8/TheEye.FeatureStore).

`TheEye.Api` registers Kafka feature publication only when `FeatureStore.Enabled` is true; otherwise it deliberately registers a no-op publisher. The API does **not** host the durable archive writer itself.

## `TheEye.FeatureWriter`

**Implemented as a separate worker.** It supports:

- Kafka feature consumption with manual commit;
- CSV archive with locking/recovery;
- transactional PostgreSQL archive;
- quarantine publishing;
- explicit startup failure for unsupported backend/configuration;
- standalone PostgreSQL export;
- no consume-and-discard worker when archive is disabled.

Source: [TheEye.FeatureWriter/Program.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/TheEye.FeatureWriter/Program.cs).

## PostgreSQL feature archive

The current local persistence database is `theeye` on host port `5433`. Feature-archive and Orleans schemas share that local database while remaining separate schemas/operational concerns.

`run-local.sh` calls:

```text
scripts/persistence-db.sh apply-features
```

for the feature-archive schema. The dedicated integration harness calls both `apply` and `apply-features` before running archive scenarios.

Sources:
- [run-local.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/run-local.sh)
- [feature-archive-integration.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/scripts/feature-archive-integration.sh)

## Conflict/revision behavior

The archive intentionally compares revisions rather than silently accepting same-identity conflicting payloads. The repository's ordering analysis correctly treats archive conflicts as evidence of upstream nondeterminism rather than something to hide by weakening feature ranking.

Therefore: **feature archive conflict detection is not the ordering fix.** See [[21 - Current Implementation Gaps and Known Defects]].

## Synthetic dataset / acceptance harness

**Implemented and improved at the current head.** The integration script now:

- supports `FEATURE_ARCHIVE_CASE`, `FEATURE_ARCHIVE_CONFIG` and `FEATURE_ARCHIVE_CASE_FILE`;
- has a standalone CSV feature-generation scenario;
- can select circular, full circular-training, spoofing-training or a custom synthetic config;
- launches each compiled .NET host from the directory containing its DLL, so its copied `appsettings.json` is loaded;
- explicitly documents the previous failure mode where launching from repository root silently dropped `FeatureStore:Datasets` settings;
- retains crash/replay, PostgreSQL outage/recovery, CSV-vs-PostgreSQL parity and paced-vs-speed-zero scenarios;
- compares the seven selected feature datasets in the speed scenario when generated.

Source: [scripts/feature-archive-integration.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/scripts/feature-archive-integration.sh).

A new operational guide also documents generating/publishing synthetic circular/spoofing datasets and collecting, verifying and labeling CSV features for ML work.

Source: [docs/synthetic-data-feature-pipeline.md](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/docs/synthetic-data-feature-pipeline.md).

## Verification surface

`TheEye.FeatureWriterTests` contains dedicated tests for CSV durability/recovery, archive worker behavior, commit observation, PostgreSQL archive behavior and export. See [[22 - Test and Verification Surface]].
