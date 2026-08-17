---
type: drop-protocol-message
status: current
message_id: 3
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 13-14"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# Asset - DROP Message 3

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 3  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 13-14.

Financial product reference entity with ISIN, product type, class/subclass and sector metadata.

## Current Kafka routing

- `mme.drop.reference.assets`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 3 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time when the message was sent. |
| `id` | 4 | Integer | The numeric identification of the asset. |
| `name` | N/A | String | The name of the asset. |
| `description` | N/A | String | The description. |
| `isin` | N/A | String | The International Securities Identification Number.<br>An empty string means no ISIN is available for the Asset. |
| `assetType` | 1 | Char | The financial product type.<br>Supported values:<br>U = Undefined<br>B = Bond<br>b = Business trust<br>C = Currency<br>D = Daily leverage certificate<br>E = Energy<br>e = Equity<br>T = Etf<br>F = Future<br>I = Index<br>i = Investment fund<br>M = Metal<br>O = Option<br>R = Reit<br>r = Right<br>W = Warrant<br>S = Sukuk |
| `assetClassName` | N/A | String | The asset class the Asset belongs to in RDM. |
| `assetSubClassName` | N/A | String | The asset sub class the Asset belongs to in RDM. |
| `sectorCode` | N/A | String | An optional sector code for the asset. Empty string if no<br>sector code was available. |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |

## Business / surveillance data value

Underlying instrument identity (name, ISIN, product type/class, sector).

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
