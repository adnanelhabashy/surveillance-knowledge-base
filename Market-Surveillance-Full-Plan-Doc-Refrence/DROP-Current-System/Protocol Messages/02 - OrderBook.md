---
type: drop-protocol-message
status: current
message_id: 2
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 14-18"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# OrderBook - DROP Message 2

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 2  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 14-18.

Tradable/order-book reference entity including asset linkage, price/quantity conventions, ticks, status, repo attributes and limits.

## Current Kafka routing

- `mme.drop.reference.orderbooks`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 2 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time when the message was sent. |
| `id` | 4 | Integer | The numeric identification of the order book. |
| `name` | N/A | String | The name of the order book. |
| `description` | N/A | String | The description. |
| `marketId` | 4 | Integer | The numeric identification of the market this order book<br>belongs to. |
| `marketSegmentId` | 4 | Integer | The numeric identification of the market segment this<br>order book belongs to. |
| `assetId` | 4 | Integer | The numeric identification of the asset this order book<br>belongs to. |
| `assetName` | N/A | String | The name of the asset. |
| `assetType` | 1 | Char | Supported values:<br>U = Undefined<br>B = Bond<br>b = Business trust<br>C = Currency<br>D = Daily leverage certificate<br>E = Energy<br>e = Equity<br>T = Etf<br>F = Future<br>I = Index<br>i = Investment fund<br>M = Metal<br>O = Option<br>R = Reit<br>r = Right<br>W = Warrant<br>S = Sukuk |
| `strikePrice` | 8 | Long | The strike price for this order book. 0 if not an option or<br>warrant. |
| `expirationDate` | 8 | Long | The expiration date for this order book. 0 if not a<br>derivative. |
| `firstTradingDate` | 8 | Long | The first trading date for the instrument. |
| `lastTradingDate` | 8 | Long | The last trading date for the instrument. |
| `optionType` | 1 | Char | Supported values:<br>U = Undefined<br>C = Call<br>P = Put |
| `currency` | N/A | String | The ISO currency the order book is traded in, such as<br>USD. |
| `currencyUnit` | 1 | Byte | Specifies if the order book is traded in the first unit, such<br>as USD, or in the second unit, such as CENT.<br>Supported values:<br>U = Undefined<br>P = Primary<br>S = Secondary |
| `currencyRelation` | 4 | Integer | Specifies the relation between the second and the first<br>unit. For instance, if the currency is USD and the<br>currencyUnit is second, this field will be set to 100,<br>indicating that there are 100 cents on a US dollar. |
| `contractSize` | 4 | Integer | The contract size. Set to 1 for a cash product. |
| `decimalsInContractSize` | 1 | Byte | The number of implicit decimals in the contract size for<br>this order book. |
| `priceQuotationFactor` | 8 | Long | The price quotation factor used to calculate the trade<br>price from the order. |
| `decimalsInPriceQuotatio` | 1 | Byte | The number of implicit decimals in the price quotation |
| `priceType` | 1 | Char | The price type configured for the orderbook.<br>Supported values:<br>B = Basis point<br>C = Percentage of nominal<br>D = Dirty price<br>I = Yield<br>M = Price<br>Q = Point |
| `decimalsInPrice` | 1 | Byte | The number of implicit decimals in the price used for<br>trading in this order book. |
| `numberOfItems` | 2 | Short | The number of price tick items (the<br>length of the array). |
| `decimalsInStrikePrice` | 4 | Integer | The number of implicit decimals in the strike price of the<br>order book. |
| `rankingRule` | 1 | Byte | The ranking rule for order books belonging to this market<br>segment:<br>Supported values:<br>1 = Price time<br>2 = Inverted price time |
| `minimumQuantity` | 8 | Long | The minimum quantity for orders |
| `maximumQuantity` | 8 | Long | The maximum quantity for orders |
| `numberOfSettlementDay` | 4 | Integer | Number of settlement days for orderbook's market |
| `lotSize` | 8 | Long | The lot size for orders |
| `decimalsInQuantity` | 1 | Byte | The number of implicit decimals in the quantity used for<br>trading in this order book. |
| `tradable` | 1 | Boolean | Specifies if the order book allows for trading or is only<br>specified to store prices from an external source. |
| `status` | 1 | Char | Supported values:<br>A = Active<br>I = Inactive<br>S = Suspended |
| `sectorCode` | N/A | String | An optional sector code for the asset. Empty string if no<br>sector code was available. |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `primary` | 1 | Boolean | True if the order book is defined as primary for the Asset. |
| `testOrderbook` | 1 | Boolean | True if the order book is used for PRV, Production<br>Realtime Verification. |
| `hasRepoOrderbook` | 1 | Boolean | True if RepoOrderbook is part of this message. |
| `returnDate` | 8 | Long | The return date of the repo transaction |
| `referencePrice` | 8 | Long | The reference price used in the repo calculations |
| `recallAllowed` | 1 | Boolean | If recall is allowed |
| `initialAmountAlgorithm` | 1 | Byte | The algorithm used to calculate the initial cash flow<br>transfer value. |
| `returnAmountAlgorithm` | 1 | Byte | The algorithm used to calculate the return cash flow<br>transfer value. |
| `dayCountConvention` | 1 | Byte | The day count basis used in the calculations of initial and<br>return values.<br>Supported values:<br>1 = ActACTISMA<br>2 = ActACTISDA<br>4 = Act365<br>5 = Act360<br>6 = Eu30360<br>7 = Us30360<br>9 = Isda30360 |
| `haircut` | 4 | Integer | Haircut used in the calculation of cash flow transfer<br>values. Expressed with 2 decimals |
| `repoType` | 1 | Byte | Supported values:<br>10 = Repo<br>20 = Security lending<br>30 = Sell buy back |
| `listingBoard` | N/A | String | Specifies the board of listings the orderbook relates to, if<br>any. |
| `minimumValue` | 8 | Long | The minimum order value accepted for the order book,<br>configured on the market segment level. |
| `maximumValue` | 8 | Long | The maximum order value accepted for the order book,<br>configured on the market segment level. |
| `decimalsInValue` | 1 | Byte | The number of decimals used for the minimumValue and<br>maximumValue fields. |
| `additionalInfo` | N/A | String | Additional information related to an order book. Can be<br>provided in different languages. |
| `assetAdditionalInfo` | N/A | String | Additional information related to an asset the order book<br>relates to. Can be provided in different languages. |
| `decimalsInPriceQuotatio<br>nFactor` | 1 | Byte | The number of implicit decimals in the price quotation<br>factor. |
| `<PriceTick><br>group` |  |  | An array of price tick items with the valid step size given<br>a price range. |
| `start PriceTick` |  |  |  |
| `→ lowerLimit` | 8 | Long | The lower limit for which the step size applies |
| `→ upperLimit` | 8 | Long | The upper limit for which the step size applies |
| `→ stepSize` | 8 | Long | The step size |
| `end PriceTick` |  |  |  |
| `numberOfSettlementDay<br>s` | 4 | Integer | Number of settlement days for orderbook's market<br>segment |
| `<CombinationLeg><br>group` |  |  | Array of combinationLeg records.<br>Note : Set to null for a single order book. |
| `start CombinationLeg` |  |  |  |
| `→ singleOrderBookId` | 4 | Integer | The ID of the single order book |
| `→ buyLeg` | 1 | Boolean | A boolean indicating if this is the buy leg |
| `→ ratio` | 4 | Integer | The ratio between combination legs. |
| `→ priceQuotationFactor` | 8 | Long | This field only has another value than 1, if the legs<br>constitute both a derivative and the underlying security,<br>such as a BUY/WRITE. |
| `end CombinationLeg` |  |  |  |
| `<RepoOrderbook><br>component` |  |  | REPO related attributes. For non repo order books, this is<br>set to null. |
| `start RepoOrderbook` |  |  |  |
| `end RepoOrderbook` |  |  |  |

## Business / surveillance data value

Instrument/order-book metadata, price/quantity scaling, tick structure, trading status and product attributes.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
