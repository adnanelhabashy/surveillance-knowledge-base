---
type: drop-protocol-message
status: current
message_id: 19
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 11"
tags:
  - drop/message/transactional-data
  - source/nasdaq-drop
---

# Commit - DROP Message 19

**Category:** Transactional Data  
**Message group:** 31  
**Message ID:** 19  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 11.

Marks that all messages for the current transaction have been sent and carries transaction timing information.

## Current Kafka routing

- `mme.drop.parsed.commit`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 19 |
| `partitionId` | 1 | Byte | The partition used. |
| `transactionId` | 8 | Long | Identification of the new transaction. |
| `transactionTime` | 8 | Long | The transaction time, which is the time DSF assigns the<br>message when being sequenced. |
| `gatewayTime` | 8 | Long | The time when the gateway received the inbound<br>message. |
| `duration` | 8 | Long | The time (ns) spent processing the message. |

## Business / surveillance data value

End boundary for matching rounds and timing/processing-duration evidence.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
