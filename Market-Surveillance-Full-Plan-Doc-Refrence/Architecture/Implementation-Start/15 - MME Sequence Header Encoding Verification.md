---
id: IMPL-START-15
type: architecture-validation
status: verified
tags:
  - surveillance/implementation
  - surveillance/sequence
  - surveillance/source-validation
  - drop/kafka
---

# Kafka Sequence Header Encoding Verification

## Decision status

> [!IMPORTANT]
> **Verified current application contract — 2026-08-19.**
>
> `mme-sequence-number` and `topic-sequence-number` are 8-byte little-endian `UInt64` Kafka headers. `topic-sequence-epoch` is 32 lowercase hexadecimal ASCII/UTF-8 characters.

The official Nasdaq DROP protocol also uses little-endian values in its binary payload, but the Kafka headers below are **application-level metadata added by the current platform**. The Nasdaq wire specification is not the source of truth for these Kafka header names.

## Verified current Kafka header contract

| Header | Current format | THE EYE rule |
|---|---|---|
| `mme-sequence-number` | 8-byte binary unsigned `UInt64`, little-endian | Required source/global sequence metadata. Decode only as little-endian binary. |
| `topic-sequence-number` | 8-byte binary unsigned `UInt64`, little-endian | Required for the selected-topic continuity guard. Producer encoding is equivalent to `BitConverter.GetBytes(ulong)` on the current little-endian .NET runtime. |
| `topic-sequence-epoch` | UTF-8/ASCII-compatible text, exactly 32 lowercase hexadecimal characters, normally GUID `"N"` format | Required together with `topic-sequence-number`; sequence identity is `(topic, epoch, number)`. |
| `drop-group-id` | ASCII decimal text | Parse as decimal `Int16` range. |
| `drop-message-id` | ASCII decimal text | Parse as decimal `Int16` range. |
| `drop-partition-id` | ASCII decimal text | Parse as decimal byte range. |

Example topic epoch:

```text
0123456789abcdef0123456789abcdef
```

## Compatibility policy

The persistence side can accept `topic-sequence-number` as UTF-8/ASCII decimal text as a rollout compatibility fallback.

THE EYE production default is stricter:

```text
SourceAssembly:TopicSequence:AllowAsciiDecimalFallback = false
```

This avoids heuristic decoding in the surveillance hot path. If a staged producer rollout temporarily requires text compatibility, the fallback can be explicitly enabled in `appsettings.json` and disabled again after rollout.

## Why both topic values are required

Never interpret `topic-sequence-number` alone.

Correct identity:

```text
KafkaTopic
+ TopicSequenceEpoch
+ TopicSequenceNumber
```

The epoch separates sequence lifetimes/restarts. A lower number in a new epoch is a new baseline, not automatically a backward sequence fault.

The current guard keeps an independent frontier for every `(topic, epoch)` pair, which also avoids corrupting continuity if more than one producer epoch appears on a shared topic.

## Relationship to MME sequence

Keep the two sequence concepts separate:

```text
MmeSequenceNumber
    = global/source ordering and evidence across DROP message types

TopicSequenceEpoch + TopicSequenceNumber
    = continuity proof for one configured Kafka topic sequence
```

When THE EYE deliberately excludes reference topics, the MME source sequence visible to THE EYE is sparse. Therefore a jump in `MmeSequenceNumber` is **not** itself a selected-topic gap.

Selected-topic gap detection uses the per-topic sequence pair. Global MME sequence remains attached to every canonical event and is still used to preserve relative source order among selected events.

See [[16 - Trading-Only Acquisition and Topic Sequence Guard|Trading-Only Acquisition and Topic Sequence Guard]].

## Current implementation

Decoder:

```text
TheEye.Ingestion/DropSourceRecordContextFactory.cs
```

Tests cover:

- 8-byte little-endian `mme-sequence-number`;
- 8-byte little-endian `topic-sequence-number`;
- optional ASCII decimal compatibility mode;
- required number/epoch pairing;
- exact 32-character lowercase-hex epoch validation;
- malformed header rejection.

## Oracle persistence representation

The current sequence-guard summary uses:

```text
TOPIC_SEQUENCE_NUMBER NUMBER(20)
TOPIC_SEQUENCE_EPOCH  VARCHAR2(32 CHAR)
```

These widths fit the current Kafka contract. `UInt64.MaxValue` has 20 decimal digits, and the epoch is exactly 32 characters.

Oracle persistence is useful for audit/reconciliation. It is **not** used as the live gap-checking loop; the hot path checks sequence continuity directly from Kafka headers in memory.

## Source authority note

- Official Nasdaq DROP specification: authoritative for native DROP payload fields and little-endian DROP wire encoding.
- Current platform producer/header contract: authoritative for these additional Kafka headers.
- THE EYE decoder must fail closed on malformed/missing required source metadata rather than inventing values from Kafka offsets.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[14 - Data Quality and Capability Gaps|Data Quality and Capability Gaps]]
- [[16 - Trading-Only Acquisition and Topic Sequence Guard|Trading-Only Acquisition and Topic Sequence Guard]]
- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/15 - Source Classification and Reliability|Source Classification and Reliability]]
