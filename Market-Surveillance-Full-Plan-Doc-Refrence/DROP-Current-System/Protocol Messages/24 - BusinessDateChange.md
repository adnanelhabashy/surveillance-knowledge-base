---
type: drop-protocol-message
status: current
message_id: 24
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 48"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# BusinessDateChange - DROP Message 24

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 24  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 48.

Signals a business-date change.

## Current Kafka routing

- `mme.drop.parsed.businessdatechanges`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 24 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time this message was sent. |
| `marketId` | 4 | Integer | A numeric identification of the market where there was a<br>change of business date. |
| `businessDate` | 8 | Long | The business date that has started. |

## Business / surveillance data value

Business-day boundary.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
