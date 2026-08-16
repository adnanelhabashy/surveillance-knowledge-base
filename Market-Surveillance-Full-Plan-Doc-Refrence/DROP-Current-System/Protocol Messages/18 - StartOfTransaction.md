---
type: drop-protocol-message
status: current
message_id: 18
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 11"
tags:
  - drop/message/transactional-data
  - source/nasdaq-drop
---

# StartOfTransaction - DROP Message 18

**Category:** Transactional Data  
**Message group:** 31  
**Message ID:** 18  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 11.

Marks the start of a new matching transaction / matching round.

## Current Kafka routing

- `mme.drop.parsed.startoftransaction`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 18 |
| `partitionId` | 1 | Byte | The partition used. |
| `transactionId` | 8 | Long | Identification of the new transaction. |

## Business / surveillance data value

Transaction boundary for reconstructing matching rounds.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
