---
type: drop-protocol-message
status: current
message_id: 11
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 42-43"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# EquilibriumPrice - DROP Message 11

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 11  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 42-43.

Emitted when an equilibrium price is calculated, including bid/offer quantities and imbalance.

## Current Kafka routing

- `mme.drop.parsed.equilibriumprices`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 11 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `orderBookId` | 4 | Integer | The order book the equilibrium price is for. |
| `equilibriumPrice` | 8 | Long | The equilibrium price for the order book. |
| `bidQuantity` | 8 | Long | The aggregated bid quantity at the equilibrium price or<br>better. |
| `offerQuantity` | 8 | Long | The aggregated offer quantity at the equilibrium price or<br>better. |
| `bidImbalanceQuantity` | 8 | Long | If the imbalance is on the bid side this field is populated<br>with bidQuantity – offerQuantity, otherwise set to 0. |
| `offerImbalanceQuantity` | 8 | Long | If the imbalance is on the offer side this field is populated<br>with offerQuantity – bidQuantity, otherwise set to 0. |
| `sessionId` | 4 | Integer | The ID of the current session for the order book. |
| `bestBidPrice` | 8 | Long | The best bid price. |
| `bestBidQuantity` | 8 | Long | The aggregated quantity at the best bid price. |
| `bestOfferPrice` | 8 | Long | The best offer price. |
| `bestOfferQuantity` | 8 | Long | The aggregated quantity at the best offer price. |

## Business / surveillance data value

Auction context: equilibrium price, aggregate quantities and imbalance.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
