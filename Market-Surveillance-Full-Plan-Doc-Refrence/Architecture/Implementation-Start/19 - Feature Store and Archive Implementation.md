---
id: IMPL-START-19
type: implementation-reference
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
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

**Implemented.** The project contains the feature envelope/row model and supporting validation/revision logic, including:

- `FeatureEnvelopeV1`
- `FeatureEnvelopeJson`
- `FeatureEnvelopeValidator`
- `FeatureRow`
- `FeatureRowMetadata`
- `FeatureEvaluationRevision`
- `FeatureRevisionComparer`
- publisher metrics
- disabled/no-op publisher support

Source: [TheEye.FeatureStore](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.FeatureStore).

`TheEye.Api` registers Kafka feature publication only when `FeatureStore.Enabled` is true; otherwise it deliberately registers a no-op publisher. The API does **not** host the durable archive writer itself.

## `TheEye.FeatureWriter`

**Implemented as a separate worker.** Startup behavior is explicit:

- binds `Kafka` and `FeatureArchive` configuration;
- if archive is disabled, it does not start a consume-and-discard loop;
- supports exactly one selected archive backend at runtime;
- CSV backend uses locking/recovery and durable writes;
- PostgreSQL backend is transactional and requires the configured feature-archive connection string;
- unknown backend fails startup;
- registers a Kafka quarantine publisher;
- consumes features with manual commit through `FeatureArchiveWorker`;
- includes a standalone PostgreSQL `export` command.

Source: [TheEye.FeatureWriter/Program.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.FeatureWriter/Program.cs).

## PostgreSQL schema provisioning

**Implemented in the local startup path.** The audited `development` head specifically changes `run-local.sh` to run:

```text
scripts/persistence-db.sh apply-features
```

This provisions the feature archive schema needed by the PostgreSQL writer on a fresh local run.

Source: [run-local.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/run-local.sh).

Infrastructure folders include dedicated `infra/feature-archive`, plus Orleans and Redis infrastructure definitions.

## Conflict/revision behavior

The archive intentionally compares revisions rather than silently accepting same-identity conflicting payloads. The repository's own current issue analysis states that `FeatureRevisionComparer` is behaving correctly and that weakening rank/conflict detection would hide upstream nondeterminism.

Therefore: **feature archive conflict detection is not the bug.** The current defect is ordering before feature generation. See [[21 - Current Implementation Gaps and Known Defects]].

## Known acceptance-harness gap

The repository documents a separate synthetic harness problem: three circular datasets are disabled by `FeatureStoreOptions` defaults, while the integration harness launches the compiled API without explicitly supplying all dataset overrides. The intended seven-dataset acceptance run therefore needs explicit configuration.

This is a **test/harness configuration defect**, separate from the production cross-topic ordering defect.

Source: [current_issue.md](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/current_issue.md).

## Verification surface

`TheEye.FeatureWriterTests` contains dedicated tests for:

- CSV archive backend;
- partial CSV recovery;
- feature archive worker behavior;
- commit observer integration seam;
- PostgreSQL archive backend;
- PostgreSQL export.

See [[22 - Test and Verification Surface]].
