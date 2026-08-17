---
type: drop-protocol-message
status: current
message_id: 26
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 34-35"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# TradeStatistics - DROP Message 26

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 26  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 34-35.

Per-order-book trading statistics including OHLC, last price/quantity, daily quantity/value, VWAP and trade count.

## Current Kafka routing

- `mme.drop.parsed.tradestatistics`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 26 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The timestamp when the trade statistics. |
| `orderBookId` | 4 | Integer | The order book for the trade statistics. |
| `openPrice` | 8 | Long | The first trade price. |
| `highPrice` | 8 | Long | The highest trade price. |
| `lowPrice` | 8 | Long | The lowest trade price. |
| `lastPrice` | 8 | Long | The last trade price. |
| `lastAuctionPrice` | 8 | Long | The last auction trade price. |
| `lastQuantity` | 8 | Long | The last trade quantity. |
| `dailyQuantity` | 8 | Long | Daily traded quantity including reported trades. |
| `dailyTradeReportedQuant` | 8 | Long | Daily reported trade quantity. |
| `dailyValue` | 8 | Long | The daily quantity times the price. |
| `vwap` | 8 | Long | The volume weighted average price. |
| `dailyNumberOfTrades` | 8 | Long | The number of trades done during this business date. |
| `dailyTradeReportedQuant<br>ity` | 8 | Long | Daily reported trade quantity. |

## Business / surveillance data value

Ready-made market context for price/volume/VWAP and daily activity baselines.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
