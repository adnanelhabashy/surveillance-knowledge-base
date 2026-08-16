---
type: drop-business-dictionary
status: current
tags:
  - drop/business-model
  - drop/join-keys
---

# Business Data Dictionary and Join Keys

| Field / concept | Meaning | Primary messages / relationships |
|---|---|---|
| `messageGroup` | Protocol message family/group; all documented messages use group 31. | All messages |
| `messageId` | Protocol message type ID. | All messages |
| `partitionId` | DROP partition. | All messages |
| `transactionId` | Matching-round transaction boundary identity. | StartOfTransaction, Commit |
| `orderBookId` | Tradable order-book identity. | OrderBook, Order, Trade, BBO, statistics, limits, sessions, etc. |
| `assetId` | Asset/instrument identity. | Asset, OrderBook, AccountPositionUpdate |
| `participantId` | Trading member / broker participant-role identity. | Participant, Actor, Account, Order, Trade, etc. |
| `actorId` | Trading user / actor identity. | Actor, Order, Trade, RFQ, breaker records |
| `submitterId` | Actor who submitted the latest order/report change. | Order, OffExchangeTrade |
| `onBehalfOfSubmitterId` | Actor submitting on behalf of another owner. | Order, OffExchangeTrade, RFQ/IQ |
| `accountId` / `account` | Trading account identity. | Account/reference and transactional messages |
| `investorId` | Investor identity linked from account reference data. | Account, Investor, AccountPositionUpdate |
| `custodianId` / `custodian` | Custodian identity/context. | Custodian, Orders/Trades/RFQ/IQ |
| `orderId` | Globally unique order identifier according to the spec. | Order, Trade, RejectedOrder, breaker records |
| `previousOrderId` | Previous quote order ID when re-quoting. | Order |
| `orderToken` | Client-generated order token (OUCH terminology). | Order, Trade |
| `matchId` | Uniquely identifies a match between buyer and seller. | Trade |
| `side` | Buy / Sell. | Orders, trades and quote messages |
| `orderStatus` / `orderStatusBefore` | After/before order lifecycle states. | Order |
| `changeReason` | Why the order/report update was emitted. | Order, OffExchangeTrade |
| `dealSource` | Circumstance/source of the trade: continuous, reported, same participant, auction, away market, etc. | Trade |
| `passiveAggressive` | Whether the trade side was passive, aggressive or neither. | Trade |
| `selfMatchPreventionKey` | Key used for self-match prevention. | Order |
| `tradeStatus` | New / updated / cancelled trade. | Trade |
| `sessionId` / session type | Session identity and matching phase context. | SessionChange, EquilibriumPrice, trigger orders |
| `businessDate` | Trading business-date boundary. | InitialBusinessDate, BusinessDateChange |
| `timestamp`, `timeCreated`, `timeChanged`, `tradeTime` | Exchange/feed event timing; protocol dates/timestamps are nanoseconds since epoch. | Multiple |

## Join strategy principle

Do not flatten away these source identifiers when building surveillance requirements. Preserve raw identifiers and derived resolved names separately so evidence can be traced back to the original DROP event.
