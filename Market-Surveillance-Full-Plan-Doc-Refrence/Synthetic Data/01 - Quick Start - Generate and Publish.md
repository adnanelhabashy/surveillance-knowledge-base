---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Quick Start - Generate and Publish

Everything runs through one CLI: `TheEye.SyntheticData`. Run it from the repo
root (`the-eye-v2/`).

## Generate a dataset

```bash
dotnet run --project TheEye.SyntheticData -- generate \
  --config TheEye.SyntheticData/config/circular-training.json \
  --output artifacts/synthetic/circular-training
```

- `--output` is **optional** — omit it and the dataset lands in
  `artifacts/synthetic/<runId>/` (the `runId` from the config, sanitized).
- If the output folder already has files, the run **fails**; add `--force` to replace.
- Prints a manifest summary at the end (event / trade / label counts, difficulty split).

## Where the data is stored

**On disk, inside the repo, under `artifacts/`** — four files per run:

```
the-eye-v2/
└── artifacts/synthetic/circular-training/
    ├── events.jsonl    ← the market data (what goes to Kafka)
    ├── labels.jsonl    ← ground truth (never goes to Kafka)
    ├── manifest.json   ← counts + reproducibility metadata
    └── config.json     ← the resolved config, saved for reproduction
```

`artifacts/` is not committed to git — datasets are disposable and regenerable
from `(seed, runId, config)`.

## Publish to Kafka

Prereq: Kafka running (e.g. `theeye-kafka` docker container) and topics created
(`surv.drop.canonical.v1`, `surv.trades.matched.v1`).

Publish an existing dataset:

```bash
dotnet run --project TheEye.SyntheticData -- publish \
  --input artifacts/synthetic/circular-training
```

Or generate + publish in one command:

```bash
dotnet run --project TheEye.SyntheticData -- generate \
  --config TheEye.SyntheticData/config/circular-training.json --publish
```

Options (both commands):

| Flag | Default | Meaning |
|---|---|---|
| `--bootstrap-servers` | `localhost:9092` | Kafka broker |
| `--canonical-topic` | `surv.drop.canonical.v1` | topic for envelopes |
| `--trades-topic` | `surv.trades.matched.v1` | topic for matched trades |
| `--speed N` | `0` | replay at N× event-time speed; `0` = as fast as possible |

## Replay through the real surveillance pipeline

```bash
# terminal 1 — silo + HTTP API (port 5175)
dotnet run --project TheEye.Api

# terminal 2 — Kafka consumer → grains
dotnet run --project TheEye.SiloConsumer

# terminal 3 — push the dataset
dotnet run --project TheEye.SyntheticData -- publish --input <dataset-dir>
```

Or skip the .NET publisher and use the generic replayer (same file format):

```bash
scripts/replay-events.sh artifacts/synthetic/circular-training/events.jsonl
```

Then inspect state: `http://localhost:5175/books/<id>/snapshot`, `/traders/<id>`,
`/alerts/<caseId>`, dashboard at `/dashboard`.

> [!WARNING]
> **Large replays vs the dedup window.** Grains dedup by lineage event id over
> only the **most recent 1,024 events per book** (in-memory state, bounded).
> Re-publishing a large dataset into a *live* silo therefore **double-applies**
> everything older than that window. For a clean re-run: restart the silo, or
> generate with a new `runId`. See [[06 - Determinism and Scale]].

## Commands cheat sheet

```text
generate --config <file> [--output <dir>] [--force] [--publish ...]
publish  --input <events.jsonl | dataset-dir> [...kafka options]
--help
```
