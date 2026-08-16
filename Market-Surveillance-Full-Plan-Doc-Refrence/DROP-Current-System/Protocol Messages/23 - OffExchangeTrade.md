---
type: drop-protocol-message
status: current
message_id: 23
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 30-32"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# OffExchangeTrade - DROP Message 23

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 23  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 30-32.

Represents one side of a reported, manually matched trade.

## Current Kafka routing

- `mme.drop.parsed.offexchangetrades`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 23 |
| `partitionId` | 1 | Byte | The partition used. |
| `createdTime` | 8 | Long | The time when the report entered into the system. |
| `changedTime` | 8 | Long | The time when the report was changed. |
| `orderBookId` | 4 | Integer | The order book the report for. |
| `participantId` | 4 | Integer | The participant owning the report. |
| `actorId` | 4 | Integer | The actor owning the report, which is the actor entering<br>the report initially or the appointed owner in the case<br>the report was entered on behalf of someone else. The<br>owning actor never changes during the report's lifetime. |
| `submitterId` | 4 | Integer | The actor submitting the message that changed the |
| `onBehalfOfSubmitterId` | 4 | Integer | Identifies an actor submitting a report on behalf of<br>someone else. When first set, the onBehalfOfSubmitterId<br>never changes. |
| `counterPartyId` | 4 | Integer | Identifies the participant on the other side of the deal<br>according to this report. |
| `orderId` | 8 | Long | The report identification, which will be global unique<br>forever. This is the ID that NFF prints on acknowledgment<br>of successful reception.<br>OrderID is needed if the participant wants to do a<br>cancellation.<br>NOTE: the FIX tag name is SecondaryTradeID |
| `clientTradeReportId` | 50 | CharArray | A client generated off-exchange trade report identifier.<br>NOTE: the FIX tag name is TradeReportID |
| `tradeReportType` | 2 | Short | Identifier to the trade report definition in the RDM<br>related to this off exchange trade.<br>Also referred to as trade report code. |
| `side` | 1 | Byte | The side of the order book.<br>Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `price` | 8 | Long | The price of the report. |
| `quantity` | 8 | Long | The reported quantity of the deal. |
| `account` | 16 | CharArray | The account to use for this report (called exClient in the<br>external API). |
| `custodian` | 32 | CharArray | The custodian to use for this report. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |
| `changeReason` | 1 | Byte | Supported values:<br>0 = Undefined<br>1 = Order canceled by the trader<br>3 = Order traded<br>5 = Order updated by user<br>6 = New order<br>9 = Order canceled by system<br>10 = Order canceled on behalf |
| `requestedPosition` | 1 | Byte | Specifies the requested position handling, such as closing<br>or opening position.<br>Supported values:<br>0 = Default for the account<br>1 = Open<br>2 = Close<br>3 = Mandatory close<br>4 = Set to default for account - Valid only when updating<br>an order |
| `settlementDate` | 8 | Long | Specifies the settlement date as agreed between parties. |
| `timeOfAgreement` | 8 | Long | Specifies the date and time of the deal.<br>Note: in FIX, this is called TransBkdTime (an UTC<br>timestamp) |
| `tradeCategory` | 1 | Byte | Supported values:<br>1 = One sided trade report<br>2 = Quote negotiation<br>3 = Two sided trade report<br>4 = Indicative quote negotiation |

## Business / surveillance data value

Required to distinguish reported/manual deals from on-book matches and to analyze report ownership, counterparty participant, price/quantity, timing and category.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
