---
type: drop-protocol-message
status: current
message_id: 20
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 32-34"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# Trade - DROP Message 20

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 20  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 32-34.

Represents one side of an automatically or manually matched trade.

## Current Kafka routing

- `mme.drop.parsed.trades`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 20 |
| `partitionId` | 1 | Byte | The partition used. |
| `tradeTime` | 8 | Long | The timestamp when the trade was made. |
| `orderBookId` | 4 | Integer | The order book for the trade. |
| `participantId` | 4 | Integer | The trading member owning the order in this trade. |
| `actorId` | 8 | Integer | The actor related to this trade. |
| `orderId` | 8 | Long | The order identifier. |
| `clientOrderId` | 50 | CharArray | A client generated order identity, referred to as ClOrdID<br>in FIX. |
| `matchId` | 8 | Long | The id that uniquely identifies a match between a buyer<br>and a seller (deal) |
| `combinationGroupId` | 4 | Integer | The id that uniquely identifies the trades that all relates<br>to the same combination deal. |
| `orderPrice` | 8 | Long | The price of the order. |
| `tradePrice` | 8 | Long | The trade price. |
| `quantity` | 8 | Long | The traded quantity. |
| `side` | 1 | Byte | Buyer or seller. |
| `dealSource` | 2 | Short | Specifies under which circumstances the deal was made<br>(such as in continuous matching or in an auction).<br>Supported values:<br>0 = Undefined<br>1 = Two orders matched in continuous matching<br>3 = Reported Trades between different participants, that<br>is, the buyer and seller belongs to different participants<br>4 = Reported Trades by the exchange between different<br>participants<br>5 = Reported Trades for the same participant - The buyer<br>and seller belong to the same participant<br>6 = Reported Trades by the exchange for the same<br>participant<br>7 = Two orders for a standard combination order book<br>matched in continuous matching<br>20 = Two orders match in an (opening) auction<br>36 = Two orders for a Tailor Made combination order<br>book (TMC) matched in continuous matching<br>53 = Two orders matched at the Away Market |
| `passiveAggressive` | 1 | Byte | Specifies if this was the aggressive side or passive side in<br>the deal. It could also be set to neither of this.<br>Supported values:<br>0 = The passive part of the deal - An order that was<br>stored on the order book<br>1 = The aggressive part of the deal - The incoming order<br>2 = Considered neither aggressive nor passive - For<br>instance used for reported trades. |
| `account` | 16 | CharArray | The account that was used for the trade. |
| `custodian` | 32 | CharArray | The custodian that was used for the trade. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |
| `tradeStatus` | 1 | Byte | The status of the trade<br>Supported values:<br>1 = New<br>2 = Updated<br>3 = Cancelled |
| `tradeReportCode` | 1 | Byte | Specifies the code of the trade report configuration<br>parameters for the reported trade. Note that this field is<br>only applicable for trade reports. |
| `reportTime` | 8 | Long | The timestamp when the trade was reported (trade<br>reports only). |
| `orderToken` | 8 | Long | A client generated token, called Order Token in OUCH<br>and Quote Message Id in SQF.<br>In OUCH the Order Token is guaranteed to be unique per<br>actor (OUCH session), which is not the case for SQF. |
| `hasRepoInformation` | 1 | Boolean | True if RepurchaseAgreementInformation is part of this<br>message. |
| `repoType` | 1 | Byte | Supported values:<br>10 = Repo<br>20 = Security lending<br>30 = Sell buy back |
| `returnDate` | 8 | Long | The return date of the repo transaction |
| `referencePrice` | 8 | Long | The reference price used in the repo calculations |
| `initialAmount` | 8 | Long | The initial cash flow transfer value. |
| `returnAmount` | 8 | Long | The return cash flow transfer value. |
| `recallAllowed` | 1 | Boolean | If recall is allowed |
| `haircut` | 8 | Long | Haircut used in the calculation of cash flow transfer<br>values. |
| `exchangeOrderType` | 4 | Integer | The exchange order type for the trade.<br>Supported values:<br>0 = Undefined<br>2 = Short sell<br>4 = Market bid<br>8 = Price stabilization<br>128 = Margin |
| `counterPartyActorId` | 4 | Integer | Identifier to the actor that is the owner of the opposite<br>order to this trade. |
| `excludedFromVWAP` | 1 | Boolean | Specifies if the trade is excluded from VWAP calculation<br>manually, excluded = true. |
| `<RepurchaseAgreementI<br>nformation><br>component` |  |  | REPO related attributes. For non repo trades, this is is set<br>to null. |
| `start RepurchaseAgreementInformation` |  |  |  |
| `end RepurchaseAgreementInformation` |  |  |  |

## Business / surveillance data value

Primary execution evidence: match identity, order identity, actor/participant/account/custodian, price/quantity, side, deal source, passive/aggressive role, trade status, order token and counterparty actor.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
