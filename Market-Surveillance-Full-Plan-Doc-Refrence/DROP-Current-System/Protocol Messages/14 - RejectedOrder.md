---
type: drop-protocol-message
status: current
message_id: 14
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 29-30"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# RejectedOrder - DROP Message 14

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 14  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 29-30.

Summarizes a rejected order insert/update/cancel or rejected MassQuote with submitted values and error code.

## Current Kafka routing

- `mme.drop.parsed.rejectedorders`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 14 |
| `partitionId` | 1 | Byte | The partition used. |
| `rejectTime` | 8 | Long | A timestamp when the order or mass quote action was<br>rejected by the matching engine.<br>Note: A rejection which was made already by a gateway<br>does not show here. |
| `actorId` | 8 | Integer | Actor ID of the owner of the order or mass quote. |
| `orderId` | 8 | Long | Order ID (only populated for order update/cancel). |
| `orderBookId` | 4 | Integer | Submitted order-book ID. |
| `side` | 1 | Byte | Submitted side.<br>Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `price` | 8 | Long | Submitted price. |
| `quantity` | 8 | Long | Submitted quantity. |
| `errorCode` | 4 | Integer | Error code indicating the reason for the reject. |

## Business / surveillance data value

Useful for attempted behavior that never became an active order, including the rejection reason.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
