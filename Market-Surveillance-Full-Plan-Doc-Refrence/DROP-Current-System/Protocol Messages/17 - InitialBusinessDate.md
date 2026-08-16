---
type: drop-protocol-message
status: current
message_id: 17
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 49"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# InitialBusinessDate - DROP Message 17

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 17  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 49.

Reports the initial business date when the system starts in a 24/7 context.

## Current Kafka routing

- `mme.drop.parsed.initialbusinessdates`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 17 |
| `partitionId` | 1 | Byte | The partition used. |
| `businessDate` | 8 | Long | The initial business date. |

## Business / surveillance data value

Initial business-day anchor.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
