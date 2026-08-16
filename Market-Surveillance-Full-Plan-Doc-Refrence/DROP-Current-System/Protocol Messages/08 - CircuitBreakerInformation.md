---
type: drop-protocol-message
status: current
message_id: 8
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 35-37"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# CircuitBreakerInformation - DROP Message 8

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 8  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 35-37.

Emitted when a circuit breaker trips, with the affected order book, triggering orders/conditions and resulting session sequence.

## Current Kafka routing

- `mme.drop.parsed.circuitbreakerinfo`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 8 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Timestamp when the circuit breaker was triggered. |
| `orderBookId` | 4 | Integer | The order book id where the circuit breaker has tripped. |
| `hasCBIncomingOrder` | 1 | Boolean | True if message includes CircuitBreakerIncomingOrder. |
| `actorId` | 4 | Integer | Owner of the order |
| `orderId` | 8 | Long | The order ID |
| `price` | 8 | Long | Order’s limit price |
| `quantity` | 8 | Long | Remaining order quantity |
| `side` | 1 | Byte | Set to 1 for buy and 2 for sell<br>Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `hasCBHitOrder` | 1 | Boolean | True if message includes CircuitBreakerHitOrder. |
| `hasCBTriggerDetail` | 1 | Boolean | True if message includes CircuitBreakerTriggerDetail. |
| `matchPrice` | 8 | Long | The attempted match price |
| `matchQuantity` | 8 | Long | The attempted match quantity |
| `hasCBTriggerCondition` | 1 | Boolean | True if message includes CircuitBreakerTriggerCondition. |
| `upperLimit` | 8 | Long | Circuit Breaker upper limit after this event. |
| `lowerLimit` | 8 | Long | Circuit Breaker lower limit after this event. |
| `sessionSequenceId` | 4 | Integer | The ID of the session sequence that was entered because<br>of the triggered circuit breaker. |
| `<CircuitBreakerIncoming<br>Order><br>component` |  |  |  |
| `start CircuitBreakerIncomingOrder` |  |  |  |
| `end CircuitBreakerIncomingOrder` |  |  |  |
| `<CircuitBreakerHitOrder><br>component` |  |  | Circuit Breaker Hit Order Info Record.<br>Set to null if not applicable. |
| `start CircuitBreakerHitOrder` |  |  |  |
| `end CircuitBreakerHitOrder` |  |  |  |
| `<CircuitBreakerTriggerDet<br>ail><br>component` |  |  | The circuit breaker trigger details.<br>Set to null if not applicable. |
| `start CircuitBreakerTriggerDetail` |  |  |  |
| `end CircuitBreakerTriggerDetail` |  |  |  |
| `<CircuitBreakerTriggerCo<br>ndition><br>component` |  |  | The triggering condition.<br>Set to null if not applicable. |
| `start CircuitBreakerTriggerCondition` |  |  |  |
| `end CircuitBreakerTriggerCondition` |  |  |  |

## Business / surveillance data value

Direct evidence of breaker events and, where populated, the incoming/hit orders and trigger detail.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
