---
id: IMPL-START-15
type: architecture-validation
status: phase-0-required
tags:
  - surveillance/implementation
  - surveillance/sequence
  - surveillance/source-validation
  - drop/kafka
---

# MME Sequence Header Encoding Verification

## Decision status

> [!IMPORTANT]
> The byte encoding of the Kafka header `mme-sequence-number` is currently **UNVERIFIED** in this vault.

Do **not** assume that the Kafka header is big-endian or little-endian until it is verified against the running MME.Drop.Ingestor implementation or a real captured Kafka record.

## What is verified

The official DROP protocol uses **little-endian** byte order for the DROP binary payload itself.

See [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]].

That protocol-wide statement applies to the DROP wire payload and does **not**, by itself, prove the byte encoding used for the separately-added Kafka header `mme-sequence-number`.

The current surveillance baseline also establishes that `MmeSequenceNumber` is not a normal DROP payload field. It is transported separately as source metadata in Kafka headers.

See [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]].

## What is not verified

The current vault does not establish whether `mme-sequence-number` is encoded as:

```text
8-byte big-endian unsigned integer
8-byte little-endian unsigned integer
UTF-8 / ASCII unsigned decimal text
another producer-specific representation
```

[[DROP-Current-System/Services/MME Drop Ingestor|MME Drop Ingestor]] documents the sequence/checkpoint model and sparse filtered outputs, but it does not define the Kafka header byte order.

Therefore THE EYE must not let an assumed endian choice drive source-sequence decoding.

## Implementation rule

Until Phase 0 proves the encoding:

- do not hard-code `BinaryPrimitives.ReadUInt64BigEndian(...)` as an architectural assumption;
- do not hard-code `BinaryPrimitives.ReadUInt64LittleEndian(...)` as an architectural assumption;
- do not infer the header encoding from the DROP payload's little-endian rule;
- do not invent a sequence value from Kafka offset or another field when the header cannot be decoded;
- treat an undecodable or missing source sequence as a source metadata/data-quality condition.

Once the producer encoding is verified, the decoder must use one explicit deterministic format and be protected by fixture-based unit tests. Avoid heuristic endian auto-detection in production because both byte orders can produce syntactically valid `ulong` values.

## Phase-0 verification procedure

Use at least one controlled live/high-volume DROP session and perform the following:

1. Capture a real Kafka record from an authoritative current DROP topic.
2. Dump the raw bytes of the `mme-sequence-number` Kafka header without converting them first.
3. Record the Kafka topic, partition, offset, message group, message id and DROP partition for evidence.
4. Decode the same raw header bytes as:
   - unsigned 64-bit big-endian;
   - unsigned 64-bit little-endian;
   - unsigned decimal text if the bytes are valid text.
5. Compare the plausible decoded value with the corresponding ingestor Redis checkpoint:

```text
mme.drop.ingestor:{instance}:next_mme_sequence_number
```

6. Remember that the checkpoint stores the **next** source sequence to request/process, so compare using the documented checkpoint timing rather than expecting exact equality for every sampled message.
7. Preferably inspect the actual current `MME.Drop.Ingestor` C# producer code and identify the exact code that creates the `mme-sequence-number` Kafka header.
8. Capture several consecutive records, not only one, and verify that decoded sequence progression matches the expected global source-sequence behavior.
9. Save a small real fixture containing the raw header bytes and expected decoded value.
10. Add a unit test to THE EYE that proves the chosen decoder against that fixture.

## Acceptance criteria

This item becomes **VERIFIED** only when at least one of the following authoritative implementation proofs exists:

- current MME.Drop.Ingestor producer code explicitly shows how `mme-sequence-number` is serialized; or
- captured current Kafka header bytes plus Redis/source-sequence evidence unambiguously prove the encoding.

Preferred completion evidence is **both** producer-code inspection and a real Kafka fixture.

Record the result here as:

```text
Verified encoding: <format>
Verified against: <producer commit / fixture / environment>
Verification date: <date>
Fixture location: <path>
Decoder test: <test name/path>
```

## Relationship to source authority

Follow [[DROP-Current-System/15 - Source Classification and Reliability|Source Classification and Reliability]]:

- official Nasdaq DROP protocol documentation is authoritative for DROP wire/message semantics;
- verified current service/architecture documentation is authoritative for what the current system actually runs;
- implementation assumptions must not be promoted to verified facts without evidence.

The DROP protocol's little-endian rule and the Kafka `mme-sequence-number` header encoding are therefore two separate concerns.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[02 - Canonical Event Contract|Canonical Event Contract]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[14 - Data Quality and Capability Gaps|Data Quality and Capability Gaps]]
- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/Services/MME Drop Ingestor|MME Drop Ingestor]]
- [[DROP-Current-System/15 - Source Classification and Reliability|Source Classification and Reliability]]
