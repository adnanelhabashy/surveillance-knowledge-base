---
type: drop-protocol-message
status: current
message_id: 38
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 50"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# AccountPositionUpdate - DROP Message 38

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 38  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 50.

Position quantity update for accounts/investors, keyed by asset/participant/account/investor.

## Current Kafka routing

- `mme.drop.parsed.accountpositionupdates`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 38 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `assetId` | 4 | Integer | The asset identifier for this position update. |
| `participantId` | 4 | Integer | The participant identifier (if any) for this position update. |
| `accountId` | 4 | Integer | The account identifier (if any) for this position update. |
| `accountName` | N/A | String | The account name (if any) for this position update. |
| `investorId` | 4 | Integer | The investor identifier (if any) for this position update. |
| `availableLongQty` | 8 | Long | The available position long quantity. |
| `availableLoanQty` | 8 | Long | The available position loan quantity. |
| `decimalsInQuantity` | 1 | Byte | The number of decimals used for availableLongQty and<br>availableLoanQty. |

## Business / surveillance data value

Position/availability context at asset-participant-account-investor level.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
