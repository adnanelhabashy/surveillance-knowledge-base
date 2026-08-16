---
type: drop-protocol-message
status: current
message_id: 6
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 12"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# EndOfReferenceData - DROP Message 6

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 6  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 12.

Marks completion of the initial static reference-data publication at start-up.

## Current Kafka routing

- `mme.drop.parsed.endofreferencedata`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 6 |
| `partitionId` | 1 | Byte | The partition used. |

## Business / surveillance data value

Readiness boundary: initial static reference data has completed before live flow.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
