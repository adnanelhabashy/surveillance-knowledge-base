---
type: drop-protocol-message
status: current
message_id: 1
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 23-29"
tags:
  - drop/message/order-and-trade
  - source/nasdaq-drop
---

# Order - DROP Message 1

**Category:** Order and Trade  
**Message group:** 31  
**Message ID:** 1  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 23-29.

Signals an update for an order, quote or bait, carrying the order lifecycle, ownership, price/quantity, status and change reason.

## Current Kafka routing

- `mme.drop.parsed.orders`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 1 |
| `partitionId` | 1 | Byte | The partition used. |
| `timeCreated` | 8 | Long | The time when the order was entered into the system<br>(which may be many days ago). |
| `timeChanged` | 8 | Long | The time when the order was changed, for instance<br>matched. |
| `orderBookId` | 4 | Integer | Order book identifier for the order. |
| `triggerOrderBookId` | 4 | Integer | The order book the order will trigger on. Only applicable |
| `participantId` | 4 | Integer | The trading member owning the order. |
| `actorId` | 4 | Integer | The actor owning the order or quote, which is the actor<br>entering the order/quote initially or the appointed owner<br>in the case the order was entered on behalf of someone<br>else. The owning actor never changes during the order’s<br>lifetime |
| `submitterId` | 4 | Integer | The actor submitting the message that changed the order<br>or quote. This can be the same as actorId if entering<br>order/quote on own account or the same as<br>onBehalfOfSubmitterId if not entered by the owner. The<br>submitterId is the last entered a message that operated<br>on the order/quote. |
| `onBehalfOfSubmitterId` | 4 | Integer | Identifies an actor submitting an order message or quote<br>on behalf of someone else. When first set the<br>onBehalfOfSubmitterId never changes. |
| `orderId` | 8 | Long | The order identification, which will be global unique for<br>ever. |
| `previousOrderId` | 8 | Long | The previous order identification. This is only applicable<br>for quotes and will be populated with the orderId of the<br>replaced quote when re-quoting. An updated single<br>order will always keep its orderId. |
| `clientOrderId` | 50 | CharArray | A client generated order identity, referred to as ClOrdID<br>in FIX. |
| `side` | 1 | Byte | Supported values:<br>0 = Undefined<br>1 = Buy<br>2 = Sell |
| `price` | 8 | Long | The price of the order. Set to minimum long value for a<br>market order. |
| `originalQuantity` | 8 | Long | The initial quantity of the order when it was entered. |
| `orderQuantity` | 8 | Long | The initial quantity of the order when it was entered but<br>can be changed later if the order is amended. Note, that<br>the order quantity is not changed when an order is<br>matched. |
| `leavesQuantity` | 8 | Long | The remaining/open quantity of the order. |
| `displayQuantity` | 8 | Long | The actual display quantity of a reserve order at the end<br>of the transaction. Set to Long.MIN_VALUE if not a<br>reserve order. |
| `refreshQuantity` | 8 | Long | The initial display quantity of a reserve order. Set to<br>Long.MIN_VALUE if not a reserve order. |
| `minimumQuantity` | 8 | Long | The minimum quantity that the order can trade.<br>It is possible for the minimum quantity restriction to be<br>fulfilled matching several opposite orders. |
| `minimumExecution` | 1 | Boolean | Set to true if the order have minimum execution<br>restrictions. This means that the order is only allowed to<br>match/fulfill its minimum execution restriction with one<br>opposite order. (WON - Whole Or None) |
| `matchedQuantity` | 8 | Long | The matched quantity within the transaction resulting in<br>the order being updated |
| `timeInForce` | 1 | Byte | Defines the validity period for the order, that is, the<br>amount of time an order will remain in the order book if<br>not fully matched.<br>Supported values:<br>1 = Rest-Of-Day - The order will expire at the end of the<br>day it was entered<br>2 = Good-Till-Cancel - The order will rest on the order<br>book until it is cancelled or fully executed<br>3 = Immediate-Or-Cancel - In continuous matchning the<br>order will execute upon reception but never added to the<br>order book. In an action period it will be added to the<br>order book in order to participate in the next uncross<br>4 = Fill-Or-Kill - In continuous matching the order will<br>execute upon reception but never added to the order<br>book. the difference compared to Immediate-And-Cancel<br>is the this order must execute in full.<br>5 = Good-Till-Session - The order will expire at the end of<br>the specified session. This TiF will utilise the addition<br>field for defining the session id<br>6 = Number-Of-Days - The order will expire after the<br>defined number of days where the day the order is<br>entered is counted as one day. This TiF will utilise the<br>addition field for defining the number of day |
| `timeInForceData` | 1 | Short | This field is used for the time validities that requires<br>additional information.<br>For Good-Til-Session the Session Type Number for the<br>session after which the order will expire. For Number-Of-<br>Days the number of days after which the order will<br>expire. |
| `orderType` | 1 | Byte | Supported values:<br>1 = Limit - The order must execute within a limit price<br>2 = Market - The order can execute at any price<br>3 = Market-To-Limit - The order initially executes as a<br>market order and if possible to execute and if there is<br>remaining quantity, the order is converted to a limit<br>order with the execution price as the limit<br>4 = Best-Order - The order is entered without a price and<br>if there is a best price on the same side of the order book<br>(BBO), the order will be given this price and added to the<br>order book.<br>5 = Imbalance - Imbalance orders only participate in<br>auctions and only of there is a surplus on the same side<br>as the imbalance order. Imbalance orders are not part of<br>the EP calculation. |
| `initialOrderType` | 1 | Byte | Defines the initial order type of an order if not the same |
| `exchangeOrderType` | 4 | Integer | The exchange order type, such as Short Sell or Session<br>State Order.<br>Supported values:<br>0 = Undefined<br>2 = Short sell<br>4 = Market bid<br>8 = Price stabilization<br>128 = Margin |
| `orderCategory` | 1 | Byte | The order category, such as Order, Quote, Bait.<br>Supported values:<br>0 = Undefined<br>1 = Order<br>4 = Quote<br>8 = Bait - Bait generated for a combination order<br>16 = Combination leg order - Combination leg order<br>generated when a combination order matches in the leg<br>order books |
| `account` | 16 | CharArray | The account to use for this order (called exClient in the<br>external API). |
| `custodian` | 32 | CharArray | The custodian to use for this order. |
| `customerInfo` | 15 | CharArray | This field is a free text field filled in by the client entering<br>the transaction. |
| `changeReason` | 1 | Byte | The reason for sending this message. |
| `triggerCondition` | 1 | Byte | Specifies what price to compare the trigger price with as |
| `triggerPrice` | 8 | Long | The trigger price of the order. |
| `triggerSessionType` | 1 | Byte | The trigger session type is used to specify which session<br>type to trigger on.<br>Note<br>This field is only populated for trigger on session orders. |
| `orderStatus` | 1 | Byte | The status of the order, such as if it is placed on the order<br>book or not.<br>Supported values:<br>1 = The order is stored on the order book<br>2 = The order is not stored on the order book - The order<br>is not stored on the order book, for instance an incoming<br>order that has yet not been handled or an order that has<br>been canceled.<br>3 = A trigger order that has not yet been triggered<br>4 = The order is inactive |
| `orderStatusBefore` | 1 | Byte | The status of the order before the event that caused this<br>message to be sent.<br>Supported values:<br>1 = The order is stored on the order book<br>2 = The order is not stored on the order book - The order<br>is not stored on the order book, for instance an incoming<br>order that has yet not been handled or an order that has<br>been canceled.<br>3 = A trigger order that has not yet been triggered<br>4 = The order is inactive |
| `orderBookPosition` | 4 | Integer | The ranking position when the order first was added to<br>the order book. This field will also be populated if the<br>order has been amended and as a result of this loose<br>priority. The field will be 0 (zero) if the order is updated<br>in a way that it does not loose priority, such as when the<br>order trades. |
| `reloaded` | 1 | Boolean | Indicates if the order was reloaded at start of the system<br>at a new business date. |
| `requestedPosition` | 1 | Byte | Specifies the requested position handling, such as closing<br>or opening position.<br>Supported values:<br>0 = Default for the account<br>1 = Open<br>2 = Close<br>3 = Mandatory close<br>4 = Set to default for account - Valid only when updating<br>an order |
| `selfMatchPreventionKey` | 4 | Integer | Used for self match prevention |
| `orderToken` | 8 | Long | A client generated token, called Order Token in OUCH |
| `pegType` | 1 | Byte | How the price shall be pegged for an order.<br>Supported values:<br>0 = Undefined<br>1 = Pegged to Primary (same side of the order book)<br>2 = Pegged to market (opposite side of the order book)<br>3 = Pegged to mid price |
| `pegOffset` | 1 | Byte | The tick offset against the peg price in number of ticks.<br>Can both be positive and negative.<br>A positive offset value indicates a more aggressive price. |
| `capPrice` | 8 | Long | The cap price that defines the minimum/maximum<br>allowed price (most aggressive price for the order) |
| `trackedOrderbookId` | 4 | Integer | Identifier for the order book a tracking order shall track<br>the price in. |
| `orderCapacity` | 1 | Byte | Order Capacity for the order.<br>Supported values:<br>0 = Undefined<br>1 = Agency<br>2 = Proprietary<br>3 = Individual<br>4 = Principal<br>5 = Risk less principal |
| `awayMarketLocked` | 1 | Boolean | True if the away market is locked for the order book. |

## Order lifecycle codes that matter operationally

### `changeReason`

The official specification defines the following values across pp. 26-27:

| Code | Meaning |
|---:|---|
| 0 | Undefined |
| 1 | Order canceled by the trader |
| 3 | Order traded |
| 4 | Order inactivated |
| 5 | Order updated by user |
| 6 | New order |
| 7 | Market order converted / modified to equilibrium price during auction |
| 8 | Market order converted to a Limit order (MTL) |
| 9 | Order canceled by system |
| 10 | Order canceled on behalf |
| 11 | Bait re-calculated |
| 12 | Trigger order triggered and converted to an active order |
| 13 | Reserved order refreshed |
| 15 | Order canceled by system due to a price limit change |
| 17 | Linked leg canceled |
| 18 | Linked leg modified |
| 19 | Order expired due to last trading day for the order |
| 20 | Order canceled because trading is halted |
| 21 | Order inactivated because trading is halted |
| 23 | Order inactivated/purged due to corporate action |
| 24 | Rest-Of-Day order inactivated/purged |
| 25 | Inactivated due to delist |
| 26 | Other than Rest-Of-Day order inactivated/purged |
| 27 | Order inactivated/purged due to being outside purge price limits |
| 28 | Order ownership transferred |
| 29 | New inactive order |
| 30 | Order reloaded by system on new trading day |
| 34 | Order canceled after opening auction |
| 35 | Order inactivated/purged due to being outside price limits |
| 36 | Order activated after changed price limits when premium is inside new limits |
| 37 | Trigger-on-session order triggered due to session change |
| 38 | Trigger-on-session order inactivated |
| 39 | Undisclosed quantity order converted to a regular order |
| 40 | Volume-match order inactivated due to order value |
| 41 | Quote canceled because market-maker-protection delta limit was exceeded |
| 42 | Quote canceled because market-maker-protection absolute quantity limit was exceeded |
| 43 | Order deleted because trader is not allowed to trade with himself |
| 44 | Canceled by circuit breaker |
| 48 | Center-point sweep MTL order converted |
| 49 | MAQ center-point order trades below minimum quantity |
| 50 | Center-point sweep order re-loaded without MAQ and mid-tick conditions |
| 51 | Canceled due to invalid Clearing Participant Id |
| 52 | Inactivated due to T+1 |
| 53 | Activated due to T+1 |
| 54 | Canceled due to not enough volume |
| 55 | Canceled due to outside price collar |
| 56 | Peg order recalculated |
| 57 | Peg order canceled due to missing external price |
| 58 | Canceled due to corporate action |
| 59 | Canceled due to Self Match handling, using Default method |
| 60 | Canceled due to suspended investor |
| 61 | Canceled due to suspended account |

### `triggerCondition`

| Code | Meaning |
|---:|---|
| 1 | Bid greater than or equal to |
| 2 | Bid less than or equal to |
| 3 | Offer greater than or equal to |
| 4 | Offer less than or equal to |
| 5 | Last Paid greater than or equal to |
| 6 | Last Paid less than or equal to |

## Incoming-order two-message rule

The official appendix says each incoming order **always generates two Order messages**: first an incoming mirror before matching-engine processing, then a processed result showing what happened. Within the matching transaction, the incoming-order mirror is sent before Order messages for other orders affected by that transaction. See [[DROP-Current-System/07 - Order and Transaction Lifecycle|Order and Transaction Lifecycle]].

## Business / surveillance data value

Primary business evidence for order-lifecycle surveillance: who owned/submitted the order, side, price, quantities, time-in-force, order type/category, change reason, previous/current status, self-match-prevention key, order token and timestamps.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
