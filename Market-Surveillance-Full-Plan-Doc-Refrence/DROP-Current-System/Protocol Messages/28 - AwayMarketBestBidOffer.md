---
type: drop-protocol-message
status: current
message_id: 28
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 45-46"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# AwayMarketBestBidOffer - DROP Message 28

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 28  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 45-46.

Best-bid/offer update from an away/other market.

## Current Kafka routing

- `mme.drop.parsed.awaymarketbbo`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 28 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The timestamp for the Away Market BBO update. |
| `timestampBid` | 8 | Long | The timestamp for the Bid side. |
| `bidPrice` | 8 | Long | The best away market bid price. |
| `bidQuantity` | 8 | Long | The quantity for the best bid price. |
| `timestampOffer` | 8 | Long | The timestamp for the Offer side. |
| `offerPrice` | 8 | Long | The best away market offer price. |
| `offerQuantity` | 8 | Long | The quantity for the best offer price. |
| `orderBookId` | 4 | Integer | The orderbook identifier related to the away market BBO<br>update. |
| `awayMarketIdBid` | 16 | CharArray | The identifier for the away market best bid side. |
| `awayMarketIdOffer` | 16 | CharArray | The identifier for the away market best offer side. |

## Business / surveillance data value

Cross/away-market top-of-book context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
