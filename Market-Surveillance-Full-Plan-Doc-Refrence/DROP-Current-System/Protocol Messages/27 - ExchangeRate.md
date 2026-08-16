---
type: drop-protocol-message
status: current
message_id: 27
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 45"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# ExchangeRate - DROP Message 27

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 27  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 45.

Exchange-rate update for a currency pair.

## Current Kafka routing

- `mme.drop.parsed.exchangerates`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 27 |
| `partitionId` | 1 | Byte | The partition used. |
| `currencyPair` | N/A | String | The currency pair. |
| `exchangeRate` | 8 | Long | The exchange rate. |
| `decimalsInExchangeRate` | 8 | Long | The number of decimals defined for the exchange rate. |

## Business / surveillance data value

FX conversion/context where values span currencies.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
