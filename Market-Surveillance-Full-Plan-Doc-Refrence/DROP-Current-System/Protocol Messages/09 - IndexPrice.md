---
type: drop-protocol-message
status: current
message_id: 9
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 43"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# IndexPrice - DROP Message 9

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 9  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 43.

Emitted when an index price is recalculated.

## Current Kafka routing

- `mme.drop.parsed.indexprices`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 9 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `orderBookId` | 4 | Integer | The order book the index price is for. |
| `indexPrice` | 8 | Long | The index price. |

## Business / surveillance data value

Index-level price context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
