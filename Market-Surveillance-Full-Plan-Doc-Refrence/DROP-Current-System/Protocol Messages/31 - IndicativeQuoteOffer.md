---
type: drop-protocol-message
status: current
message_id: 31
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 40-42"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# IndicativeQuoteOffer - DROP Message 31

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 31  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 40-42.

Represents an offer against an indicative quote.

## Current Kafka routing

- `mme.drop.parsed.indicativequoteoffers`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 31 |
| `partitionId` | 1 | Byte | The partition used. |
| `orderBookId` | 4 | Integer | The order book identifier for the Indicative Quote Offer. |
| `status` | 1 | Byte | Supported values:<br>1 = New<br>2 = Accepted<br>3 = Cancelled by user<br>4 = Cancelled not enough quantity<br>5 = Cancelled by system<br>6 = Declined |
| `userId` | 4 | Integer | The actor/user owning the transaction. |
| `onBehalfOfSubmitterId` | 4 | Integer | The submitter of the transaction, if sent on behalf. |
| `indicativeQuoteId` | 8 | Integer | The ID of the Indicative Quote. |
| `indicativeQuoteOfferId` | 8 | Long | The ID of this Indicative Quote Offer. |
| `side` | 1 | Byte | Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `price` | 8 | Long | The price of this Indicative Quote. |
| `quantity` | 8 | Long | The quantity of this Indicative Quote. |
| `orderCapacity` | 1 | Byte | Supported values:<br>0 = Undefined<br>1 = Agency<br>2 = Proprietary<br>3 = Individual<br>4 = Principal<br>5 = Risk less principal |
| `requestedPosition` | 2 | Short | Supported values:<br>0 = Default for the account<br>1 = Open<br>2 = Close<br>3 = Mandatory close<br>4 = Set to default for account - Valid only when updating<br>an order |
| `expirationTime` | 8 | Long | The expiration time for this offer. |
| `clientOrderId` | 50 | CharArray | A client generated identity, referred to as ClOrdID in FIX. |
| `account` | 16 | CharArray | The account to use for this offer (called exClient in the<br>external API). |
| `custodian` | 32 | CharArray | The custodian to use for this offer. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |
| `creationTime` | 8 | Long |  |

## Business / surveillance data value

Offer-side response to indicative quote activity.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
