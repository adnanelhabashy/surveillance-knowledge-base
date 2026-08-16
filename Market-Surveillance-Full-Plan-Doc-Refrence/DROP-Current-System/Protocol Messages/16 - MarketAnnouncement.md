---
type: drop-protocol-message
status: current
message_id: 16
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 47-48"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# MarketAnnouncement - DROP Message 16

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 16  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 47-48.

A market announcement with source scope and classification.

## Current Kafka routing

- `mme.drop.parsed.marketannouncements`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 16 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `sequenceNumber` | 8 | Long | A sequence number, starting at 1 every day. |
| `exchangeId` | 4 | Integer | A numeric identification of the exchange the<br>announcement is for. |
| `marketId` | 4 | Integer | A numeric identification of the market the<br>announcement is for. |
| `orderBookId` | 4 | Integer | A numeric identification of the order book the<br>announcement is for. |
| `messageInformationType` | 2 | Short | A classification of what type of announcement this is. |
| `messageSource` | N/A | String | An identification of the source of this announcement. |
| `messageTime` | 8 | Long | The date and time when this message was created and<br>sent. |
| `messagePriority` | 2 | Short | The priority of this particular message (this can be used<br>by applications to filter important news messages from<br>less important ones).<br>Supported values:<br>0 = Unknown<br>1 = Low<br>2 = Medium<br>3 = High<br>4 = Critical |
| `header` | N/A | String | A short summary of the news message (80 characters is<br>maximum). |
| `documentUrl` | N/A | String | An optional URL link, where more information can be<br>provided. |
| `message` | N/A | String | The complete news message text (800 characters is<br>maximum). |
| `actorId` | 4 | Integer | The actor identification number for the user who created<br>this market announcement (E.g., this could be a user<br>within the business operations team of the marketplace). |

## Business / surveillance data value

Official market/company notice context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
