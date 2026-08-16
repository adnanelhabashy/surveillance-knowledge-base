---
type: drop-protocol-message
status: current
message_id: 22
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 42"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# BestBidOffer - DROP Message 22

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 22  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 42.

Emitted when best bid/offer price or quantity changes for an order book.

## Current Kafka routing

- `mme.drop.parsed.bestbidoffers`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 22 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `orderBookId` | 4 | Integer | The order book ID for this BBO update message. |
| `bestBidPrice` | 8 | Long | The best bid price in the market for this order book. |
| `bestBidQuantity` | 8 | Long | The quantity of the best buy order in the market for this<br>order book. |
| `bestOfferPrice` | 8 | Long | The best offer/sell price in the market for this order book. |
| `bestOfferQuantity` | 8 | Long | The quantity of the best offer/sell order in the market for<br>this order book. |

## Business / surveillance data value

Top-of-book context for order placement and market-impact analysis.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
