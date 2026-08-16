---
type: drop-protocol-message
status: current
message_id: 12
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 37-38"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# QuoteRequest - DROP Message 12

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 12  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 37-38.

Represents a quote request received by the system.

## Current Kafka routing

- `mme.drop.parsed.quoterequests`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 12 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Timestamp when the quote request was received. |
| `orderBookId` | 4 | Integer | Order book for the quote request. |
| `actorId` | 4 | Integer | The owner of the quote request. |
| `quoteRequestId` | 8 | Long | Quote request Id. |
| `side` | 1 | Byte | The side of the order book that the requester would like<br>to trade.<br>So the side selection for a request is as if the requester<br>would have put an order on this trading intent.<br>This means that responses are expected to quote on the<br>opposite side to the side given on the request.<br>Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `quantity` | 8 | Long | The requested quantity. |
| `onBehalfOfSubmitterId` | 4 | Integer | Actor who entered the request (only for on-behalf<br>requests). |
| `status` | 1 | Byte | The status of the Quote Request.<br>Supported values:<br>0 = Undefined<br>1 = New<br>2 = Updated<br>3 = Cancelled by user<br>4 = Accepted<br>5 = Cancelled by system |
| `account` | 16 | CharArray | The account to use for this quote request (called exClient<br>in the external API). |
| `custodian` | 32 | CharArray | The custodian to use for this quote request. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |

## Business / surveillance data value

RFQ intent and account context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
