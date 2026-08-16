---
type: drop-protocol-message
status: current
message_id: 21
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 38-39"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# QuoteRequestResponse - DROP Message 21

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 21  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 38-39.

Represents a response to a quote request.

## Current Kafka routing

- `mme.drop.parsed.quoterequestresponses`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 21 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Timestamp when the quote response was received. |
| `orderBookId` | 4 | Integer | Order book for the quote response. |
| `actorId` | 4 | Integer | The owner of the quote response. |
| `quoteRequestId` | 8 | Long | The quote request that this response is referring to.<br>(Only the originator of the RFQ receives responses, and<br>with response actor information as per anonymity<br>configuration). |
| `quoteResponseId` | 8 | Long | A unique quote response identifier assigned by the<br>system. |
| `side` | 1 | Byte | The side of the order book on which this quote is |
| `price` | 8 | Long | The quoted price. |
| `quantity` | 8 | Long | The quoted quantity. |
| `onBehalfOfSubmitterId` | 4 | Integer | Actor who entered the response (only for on-behalf<br>response). |
| `status` | 1 | Byte | The status of the Quote Request Response.<br>Supported values:<br>0 = Undefined<br>1 = New<br>3 = Cancelled by user<br>4 = Accepted<br>5 = Cancelled by system |
| `account` | 16 | CharArray | The account to use for this response (called exClient in<br>the external API). |
| `custodian` | 32 | CharArray | The custodian to use for this response. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |

## Business / surveillance data value

RFQ response price/quantity and actor context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
