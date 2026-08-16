---
type: source-catalog
title: Trading Surveillance Catalog — 540 cases
tags:
  - surveillance/source
---

# Trading Surveillance Catalog — 540 cases

This is the source catalog used to seed this Obsidian project. The project notes preserve its case names and descriptions. Family/detector assignments and related-case links are **initial graph organization**, not additional legal claims.

---

# Stock-Market Trading Fraud, Manipulation & Surveillance Case Catalog

> **Revised scope:** This version is deliberately focused on **trading and market-surveillance behavior** rather than every possible form of securities fraud. It covers suspicious or abusive behavior involving orders, trades, prices, volume, order books, auctions, accounts, traders, investors, brokers, venues, derivatives, short selling, settlement, securities lending, trade reporting, confidential order information, insider dealing, cross-market/cross-product activity, and manipulation-linked account fraud.
>
> **Removed from the old mixed catalog:** primarily accounting/financial-statement fraud, proxy-only misconduct, broad corporate reporting fraud, ordinary investment-adviser fee abuse, research-compensation conflicts that do not themselves involve trading, and other non-trading fraud domains.
>
> **Important:** A surveillance alert is **not proof of fraud or market abuse**. The legal test, required intent, permitted market practices, and evidentiary standard vary by jurisdiction.

## SMARTS / Nasdaq Coverage Note

Nasdaq currently states that **Nasdaq Market Surveillance (SMARTS) has 70+ preconfigured alerts**, while **Nasdaq Trade Surveillance covers 300+ behaviors across 200+ markets**. Nasdaq does **not publicly disclose the complete proprietary names and logic of every alert/behavior**. Therefore this file does **not invent a fake “complete SMARTS alert list.”**

Instead, it does two things:

1. Captures the behaviors and surveillance capabilities Nasdaq has **publicly named or described**.
2. Builds a much broader trading-surveillance catalog from the original file plus regulator-recognized and implementation-level variants that a SMARTS-like exchange surveillance system may need to monitor.

### Behaviors/capabilities publicly described by Nasdaq SMARTS materials

- **Market manipulation** — Nasdaq states SMARTS detects patterns of market manipulation.
- **Insider trading** — Nasdaq states SMARTS detects insider-trading patterns.
- **Spoofing** — Nasdaq explicitly describes spoofing scenarios that track orders and order-book impact.
- **Layering / order-book layering** — Nasdaq has publicly identified layering as a manipulation monitored with SMARTS.
- **Order-book manipulation** — Nasdaq describes visualization and surveillance of order-book manipulation.
- **Cross-market surveillance / manipulation** — Nasdaq describes cross-market surveillance capabilities.
- **Cross-asset surveillance / manipulation** — Nasdaq describes cross-asset surveillance capabilities.
- **Multi-venue surveillance** — Nasdaq describes activity across venues and related markets.
- **Cross-product manipulation** — Nasdaq Trade Surveillance explicitly highlights cross-product manipulation.
- **Wash trading** — Nasdaq Trade Surveillance publicly discusses wash-trading surveillance.
- **RFQ front running** — Nasdaq Trade Surveillance explicitly mentions RFQ frontrunning for fixed income.
- **Benchmark-fix abuse** — Nasdaq Trade Surveillance explicitly mentions surveillance for benchmark fixes.
- **Trading with knowledge** — Nasdaq Trade Surveillance explicitly mentions trading-with-knowledge surveillance.
- **Unusual pricing** — Nasdaq Trade Surveillance explicitly mentions unusual-pricing surveillance.
- **Price manipulation** — Nasdaq surveillance materials publicly discuss price-manipulation detection and investigation.
- **Marking the close** — Nasdaq surveillance materials and conference examples publicly discuss marking-the-close.
- **Abnormal market-event / anomaly detection** — Nasdaq describes AI/ML analysis of abnormal events and anomaly detection in SMARTS.

## Full Trading-Surveillance Catalog

**Total distinct scenarios in this revision: 540**

The list intentionally includes both broad abuse families and narrower implementation variants. Two related entries may therefore share the same underlying abuse concept but require different surveillance logic, time windows, data sources, products, or alert calibration.

1. **Spoofing** — Placing orders without genuine intent to execute them to create false buying or selling pressure.
2. **Layering** — Placing multiple deceptive orders at several price levels to create the appearance of strong supply or demand.
3. **Wash Trading / Wash Sales** — Buying and selling the same security without meaningful change in beneficial ownership to create fake activity or volume.
4. **Matched Orders / Matched Trading** — Two cooperating parties place matching buy and sell orders to manufacture trades, volume, or price movement.
5. **Prearranged Trading** — Parties secretly agree beforehand on the price, quantity, or timing of trades rather than trading competitively.
6. **Circular Trading** — Securities are passed through several related accounts and eventually return to the original beneficial owner to create artificial volume.
7. **Self-Trading** — The same beneficial owner trades against itself, potentially creating misleading volume or price signals.
8. **Painting the Tape** — Coordinated transactions are executed to make a security appear more actively traded or to create an artificial price trend.
9. **Marking the Close** — Trading near market close to artificially influence the official closing price.
10. **Marking the Open** — Trading around the opening auction to artificially influence the opening price.
11. **Closing-Auction Manipulation** — Entering, cancelling, or executing orders specifically to distort the closing auction result.
12. **Opening-Auction Manipulation** — Manipulating orders during the opening auction to distort the opening equilibrium price.
13. **Ramping** — Aggressively buying or selling to push a security’s price progressively in a desired direction.
14. **Price Ramping / Price Run-Up** — Repeated transactions artificially increase a share’s market price before the manipulator sells.
15. **Price Depressing** — Repeated selling activity is used to artificially drive a security’s price downward.
16. **Momentum Ignition** — Creating aggressive activity intended to trigger other traders or algorithms into continuing a price move.
17. **Mini-Manipulation** — Manipulating the underlying stock with relatively small trades to profit from a larger related position such as options.
18. **Odd-Lot Manipulation** — Using unusually small trades or orders to influence quotations, reference prices, or other market behavior.
19. **Reference-Price Gaming** — Manipulating trades or orders to influence a reference price used elsewhere in the market.
20. **Pegging Manipulation** — Trading to keep a security artificially fixed around a desired price.
21. **Price Capping** — Repeatedly selling or offering stock to prevent its price from rising beyond a chosen level.
22. **Price Flooring** — Repeated buying is used to prevent a security from falling below a chosen price.
23. **Artificial Price Maintenance** — Trading specifically to maintain a security at a price unsupported by genuine supply and demand.
24. **Quote Stuffing** — Sending and cancelling very large numbers of orders to overload, confuse, or distort the visible market.
25. **Order Stuffing** — Flooding the order book with excessive orders, frequently followed by rapid cancellation.
26. **Excessive Order Cancellation Manipulation** — Entering large quantities of orders primarily to affect other participants and then cancelling them.
27. **Flickering Orders** — Rapidly placing, modifying, and cancelling orders in ways intended to create misleading order-book signals.
28. **Phantom Liquidity** — Displaying apparent liquidity that the trader has no genuine intention of providing.
29. **Liquidity Mirage** — Coordinated or rapidly cancelled orders make the market appear substantially deeper than it really is.
30. **Order-Book Imbalance Manipulation** — Creating artificial bid/ask imbalance so other traders believe strong buying or selling pressure exists.
31. **Dominant Bid Manipulation** — Placing large deceptive bids to make demand appear stronger than it is.
32. **Dominant Offer Manipulation** — Placing large deceptive sell orders to make supply appear stronger than it is.
33. **Bid-Ask Spread Manipulation** — Manipulating orders to artificially widen, narrow, or otherwise control the quoted spread.
34. **Pinging / Liquidity Detection Abuse** — Small orders are used to discover hidden liquidity or trading intentions and then exploit that information manipulatively.
35. **Crossing Manipulation** — Coordinated transactions are crossed between related parties to create misleading prices or volumes.
36. **Collusive Trading** — Two or more traders coordinate transactions to manipulate a security.
37. **Related-Account Manipulation** — Multiple accounts controlled by or connected to the same party are used to disguise coordinated activity.
38. **Nominee-Account Manipulation** — Accounts registered to other people or entities are used to conceal the real manipulator.
39. **Trader–Investor Collusion** — A trader and investor coordinate orders or trades to create artificial market behavior or transfer benefits.
40. **Broker–Client Collusion** — A broker and client cooperate to execute manipulative transactions.
41. **Group / Syndicate Manipulation** — A larger group coordinates purchases, sales, orders, or promotions to influence a stock.
42. **Cross-Broker Manipulation** — Related traders use different brokers to hide coordinated market activity.
43. **Cross-Venue Manipulation** — Activity on one trading venue is used to manipulate the price or behavior on another venue.
44. **Cross-Market Manipulation** — One market is manipulated to generate profits in another related market.
45. **Cross-Product Manipulation** — A stock is manipulated to benefit a position in a related option, ETF, future, or other instrument.
46. **Underlying-vs-Derivative Manipulation** — Trading the underlying stock artificially changes the value or settlement of derivatives.
47. **Derivative-to-Stock Manipulation** — Activity in derivatives is used to influence trading or pricing of the underlying stock.
48. **Pump and Dump** — Fraudsters promote or manipulate a stock upward and then sell their holdings into the artificially inflated demand.
49. **Ramp and Dump** — Manipulative trading rather than promotion alone is used to ramp the price upward before selling the position.
50. **Short and Distort** — A trader establishes a short position and then spreads negative or false information to drive the price down.
51. **Trash and Cash** — False negative information is spread to depress the stock before buying it cheaply.
52. **Bear Raid** — Coordinated aggressive selling and often negative information are used to force a stock sharply lower.
53. **Microcap / Penny-Stock Manipulation** — Thinly traded small-company shares are manipulated because relatively little activity can significantly move their prices.
54. **Low-Float Manipulation** — Manipulators exploit a stock with very few freely tradable shares to move its price with relatively little capital.
55. **Pump-and-Dump Through Social Media** — Social-media promotion creates artificial enthusiasm before promoters sell their holdings.
56. **Chat-Room Manipulation** — Coordinated participants use chat rooms or messaging groups to trigger artificial buying or selling.
57. **Influencer Stock Manipulation** — A person with a large following promotes a position without appropriately revealing conflicting trading intentions.
58. **Scalping Scheme** — Someone recommends a stock to others and secretly sells into the resulting price increase.
59. **Undisclosed Touting** — A promoter recommends a stock without disclosing that they are being paid to promote it.
60. **Insider Trading / Insider Dealing** — A person trades while possessing material non-public information.
61. **Insider Tipping** — An insider provides material non-public information to another person who may trade on it.
62. **Tippee Trading** — A recipient of improperly disclosed inside information trades using that information.
63. **Information Misappropriation** — Confidential market-moving information is stolen or misused for securities trading.
64. **Trading Ahead** — A broker or market professional trades for itself before executing known customer orders likely to move the market.
65. **Front Running** — Someone trades ahead of a known large customer transaction to benefit from its expected price impact.
66. **Front Running Block Orders** — A trader takes a position before executing a client’s large block order.
67. **Front Running Research** — Trading occurs before publication of a market-moving analyst recommendation or research report.
68. **Front Running Index Changes** — Confidential knowledge of upcoming index additions or removals is exploited before public announcement.
69. **Front Running Corporate Actions** — Confidential knowledge of mergers, acquisitions, dividends, tender offers, or similar events is exploited.
70. **Misuse of Client Order Information** — Confidential information about customer orders is used for another account’s benefit.
71. **Selective Disclosure Abuse** — Material information is improperly given to selected investors before being made available to the wider market.
72. **False-Rumor Manipulation** — False market-moving rumors are deliberately circulated to influence buying or selling.
73. **Misleading-News Manipulation** — Misleading company or market news is disseminated to change a security’s price.
74. **Fake Press Release Fraud** — A fabricated or materially false press release is used to influence investors.
75. **False Social-Media Information** — False or misleading posts are deliberately spread to affect securities prices.
76. **Misleading Analyst Research** — Research is intentionally distorted to influence investors or benefit hidden positions.
77. **Undisclosed Research Conflict** — Someone recommending a security conceals a financial interest or compensation connected with the recommendation.
78. **False Corporate Disclosure** — An issuer deliberately provides materially false information to the market.
79. **Material-Omission Fraud** — Important facts are intentionally omitted so that published information becomes misleading.
80. **Delayed Material Disclosure Abuse** — Material information that should be disclosed is intentionally withheld to create a misleading market.
81. **VWAP Manipulation** — Trades are designed to distort the volume-weighted average price used as a benchmark.
82. **TWAP Manipulation** — Trading is structured to distort a time-weighted average benchmark.
83. **Settlement-Price Manipulation** — Trading near the calculation window artificially influences an official settlement price.
84. **Index-Level Manipulation** — Components of an index are manipulated to influence the calculated index value.
85. **NAV Manipulation** — Securities or inputs are manipulated to alter the reported net asset value of a fund or product.
86. **Benchmark Manipulation** — Transactions or submissions are intentionally designed to distort a market benchmark.
87. **Reference-Rate Manipulation** — Inputs affecting a reference rate are deliberately distorted.
88. **Cornering the Market** — A participant obtains enough control over available securities to manipulate their price or force others into unfavorable transactions.
89. **Market Squeeze** — Control over scarce available securities is exploited to force short sellers or other participants to transact at artificial prices.
90. **Float Manipulation** — Shares available for public trading are secretly controlled or restricted to make manipulation easier.
91. **Beneficial-Ownership Concealment** — The true owner of securities or accounts is hidden to evade disclosure or surveillance.
92. **Share Parking** — Securities are temporarily placed in another person’s account to conceal ownership, control, or regulatory limits.
93. **Nominee Ownership Scheme** — Nominees hold shares for the true controller to hide concentrated ownership or coordinated trading.
94. **Hidden Control of Multiple Accounts** — One person secretly controls numerous apparently independent investor accounts.
95. **Manipulative Short Selling** — Short sales are deliberately coordinated with other deceptive activity to force prices lower.
96. **Short-Sale Mismarking** — Short sales are intentionally marked or reported incorrectly as long sales to evade applicable requirements.
97. **Locate / Borrow Misrepresentation** — False information is provided regarding the availability or borrowing of shares required for a short sale.
98. **Abusive Naked Short-Selling Scheme** — Failure-to-deliver or naked-short activity is combined with deceptive conduct to manipulate a security.
99. **Churning** — A broker excessively trades a customer’s account primarily to generate commissions rather than benefit the customer.
100. **Unauthorized Trading** — Trades are executed in a customer’s account without required authorization.
101. **Misappropriation of Customer Assets** — Customer cash or securities are taken or improperly used.
102. **Trade Allocation Fraud / Cherry Picking** — Profitable trades are unfairly allocated to favored accounts while losing trades are assigned to others.
103. **Best-Execution Manipulation** — Customer execution is intentionally worsened so another party can improperly benefit.
104. **Interpositioning Abuse** — An unnecessary intermediary is inserted into a transaction to generate hidden profits or disadvantage the customer.
105. **Excessive Markup / Markdown Fraud** — A customer is charged a deliberately unfair transaction price while the true market economics are concealed.
106. **Commission / Fee Manipulation** — Trading activity is generated mainly to create improper fees or commissions.
107. **Account-Takeover Trading** — A compromised brokerage account is used to make unauthorized trades or participate in manipulation.
108. **Synthetic-Identity Trading Accounts** — Fabricated identities are used to open accounts and conceal manipulative activity.
109. **Stolen-Identity Brokerage Accounts** — Another person’s identity is used to open or control trading accounts.
110. **Money-Mule Trading Accounts** — Accounts controlled through intermediaries are used to move proceeds or disguise the real participants.
111. **Dormant-Account Takeover** — Inactive investor accounts are compromised and then used for fraudulent trading.
112. **IPO Allocation / Spinning Abuse** — Attractive IPO allocations are improperly provided to executives or decision-makers to influence business relationships.
113. **Trade-Reporting Manipulation** — Trades are deliberately reported incorrectly to mislead the market or regulator.
114. **Delayed-Trade Reporting Abuse** — Reporting is intentionally delayed to hide the true timing or market impact of trades.
115. **False Price Reporting** — A transaction is reported at an incorrect price to distort market information.
116. **False Volume Reporting** — Reported trading volume is fabricated or misrepresented.
117. **Order-Identity Concealment** — Account, trader, or beneficial-owner identifiers are manipulated to hide who actually placed the order.
118. **Order Splitting to Evade Surveillance** — A manipulative order is broken into many smaller orders to remain below detection thresholds.
119. **Multi-Account Threshold Evasion** — Activity is distributed across many accounts to avoid surveillance limits.
120. **Broker-Hopping / Venue-Hopping Evasion** — Manipulative activity is distributed across firms or venues to prevent any single surveillance system from seeing the complete pattern.
121. **Coordinated Timing Evasion** — Related traders deliberately stagger orders and trades to make coordinated manipulation harder to detect.
122. **Beneficial-Owner Evasion** — Related accounts are structured so that apparently independent trades actually originate from the same economic owner.
123. **Spoof-and-Trade** — Fake orders are placed on one side of the book while genuine trades are executed on the opposite side.
124. **Layer-and-Trade** — Multiple deceptive layers move the market while the manipulator executes genuine orders in the favorable direction.
125. **Pump + Wash Trading** — Wash trades manufacture apparent activity while promotion attracts genuine investors.
126. **Pump + Matched Orders** — Coordinated matched trades create apparent momentum supporting a pump-and-dump campaign.
127. **Insider Trading + Market Manipulation** — Inside information is exploited while manipulative trading is simultaneously used to maximize the resulting profit.
128. **Rumor + Trading Manipulation** — A trader establishes a position, spreads false information, and trades around the resulting price movement.
129. **Cross-Product Spoofing** — Spoof orders in one security or instrument are used to profit from a genuine position in a correlated product.
130. **Closing-Price + Derivative Manipulation** — The stock’s closing price is manipulated because that price determines the value or settlement of a related derivative.
131. **Portfolio / Basket Manipulation** — Several constituent securities are manipulated together to affect the value of an index, ETF, basket, or portfolio.
132. **Coordinated Multi-Security Manipulation** — Related securities are simultaneously traded to create artificial signals that reinforce one another.
133. **Event-Driven Manipulation** — Manipulative trading is concentrated around corporate announcements, index changes, dividends, rights issues, offerings, or other known market events.
134. **Smoking** — A trader posts attractive orders to lure slower traders, then rapidly changes those orders to worse terms in an attempt to trade profitably against incoming flow.
135. **Phishing / Order-Book Phishing** — Trades or small orders are executed to uncover another participant’s hidden orders, after which the trader exploits the discovered information.
136. **Flying** — A trader or broker tells clients or other participants that genuine bids or offers exist when those prices are not backed by actual orders or trading instructions.
137. **Printing** — A trader falsely communicates that a transaction occurred at a particular price or size when no such trade actually happened.
138. **Trading-Safeguard Bypass Manipulation** — Orders or transactions are structured specifically to circumvent market controls such as volume limits, price limits, or spread protections.
139. **Illiquid Price-Setting Manipulation** — A participant deliberately creates a market price in an illiquid instrument where a small trade can artificially establish the price.
140. **Short-Window Price Spike-and-Reversal Manipulation** — Concentrated orders or transactions cause a sharp short-term price movement that quickly reverses once the manipulative activity stops.
141. **Rapid Position-Reversal Manipulation** — A trader repeatedly reverses positions over a short period while representing significant market volume and causing meaningful price movements.
142. **Portfolio Pumping** — A fund manager buys securities already owned by the portfolio near a reporting date to artificially increase their closing prices and reported performance.
143. **Window Dressing** — A portfolio manager buys recent winners or removes poor performers immediately before holdings are disclosed to create a misleading picture of strategy or performance.
144. **Advancing the Bid** — A participant repeatedly raises the displayed bid without genuine market demand in order to push the apparent market price upward.
145. **Cash-Account Free-Riding** — A customer buys and sells securities in a cash account before actually paying for the purchase with settled funds.
146. **Restricted-Stock Loan Default Scheme** — Restricted insider shares are placed with a purported stock-loan company as collateral, the loan is intentionally or suspiciously defaulted, and the stock is then sold into the market.
147. **Restrictive-Legend Removal Abuse** — Restrictions on recently issued shares are improperly removed so stock that should not yet be freely tradable can be deposited and sold publicly.
148. **Dormant Shell Hijacking** — Fraudsters gain control of an inactive public shell company, reactivate it, and use its securities as a vehicle for manipulation or promotion.
149. **Reverse/Forward Split Share-Concentration Scheme** — Reverse mergers or stock splits are structured so that most freely tradable shares become concentrated among a coordinated group capable of manipulating the stock.
150. **Trend-Hijacking Issuer Promotion** — A low-priced issuer suddenly changes its name, ticker, or supposed business toward a fashionable sector primarily to attract speculative investors.
151. **Unsupported Partnership / Joint-Venture Promotion** — An issuer announces supposedly valuable partnerships, financing arrangements, or joint ventures that cannot be independently verified and uses them to stimulate its stock price.
152. **Dormancy-Reactivation Promotion Scheme** — A previously inactive issuer suddenly launches heavy press-release, social-media, or investor-promotion activity without corresponding evidence of genuine operations.
153. **Small-Purchase Price Support for Large Inventory** — A holder of a very large position repeatedly makes small purchases mainly to support or increase the quoted price of the larger position.
154. **Deposit–Sell–Wire-Out Scheme** — Large blocks of thinly traded stock are deposited, quickly liquidated, and the cash proceeds immediately transferred out of the brokerage account.
155. **Coordinated Same-Issuer Deposit-and-Liquidation** — Apparently unrelated investors open accounts around the same time, deposit shares of the same low-priced company, and liquidate them in a coordinated manner.
156. **Omnibus-Account Liquidation Abuse** — Large quantities of low-priced securities are sold through omnibus or similar accounts that conceal the underlying beneficial accounts.
157. **Master/Subaccount Anonymity Abuse** — A master/subaccount structure is deliberately used to hide which underlying traders are responsible for coordinated or manipulative securities activity.
158. **Securities-Based Currency Conversion Scheme** — Securities such as ADRs are bought in one currency or jurisdiction and rapidly transferred or liquidated elsewhere principally to convert or move money.
159. **Mirror Trading Through Securities** — Matching or offsetting securities transactions are executed through different accounts or jurisdictions to transfer economic value or convert currencies while disguising the connection.
160. **Alternative Merger / Exchange-Offer Price Manipulation** — A bidder or related participant manipulates its own share price during a merger or exchange-offer valuation period because the price affects the exchange ratio.
161. **Pre-Offering Short-Sale / Rule 105 Abuse** — A trader shorts a company’s stock shortly before a public offering and then purchases the same stock through the offering contrary to applicable restrictions.
162. **IPO Aftermarket Tie-In Scheme** — IPO shares are allocated only if customers agree or are pressured to buy additional shares in the aftermarket, manufacturing apparent post-IPO demand.
163. **IPO Allocation-for-Excessive-Commission Scheme** — Attractive IPO allocations are effectively exchanged for excessive commissions or other undisclosed compensation paid through unrelated transactions.
164. **Artificial Aftermarket Conditioning During a Distribution** — An underwriter, issuer, or distribution participant creates or solicits artificial buying interest so aftermarket demand appears stronger than it really is.
165. **Mark-Down → Accumulate → Mark-Up → Distribute Scheme** — Coordinated accounts first push a thinly traded stock downward to accumulate, then push it upward before unloading the position.
166. **Stock-Promoter Liquidation Scheme** — A person promoting a security simultaneously or subsequently sells substantial holdings while the promotion attracts other investors.
167. **Quotation-Reactivation Manipulation** — Parties seek to initiate or resume public quotations for a dormant or transformed low-priced issuer as part of a broader promotional or manipulation campaign.
168. **Synchronized Illiquid-Stock Trading** — Multiple apparently unrelated accounts suddenly begin trading the same illiquid security at the same time, potentially revealing hidden coordination.
169. **Fake Share-Provenance / Legal-Opinion Scheme** — False purchase agreements, questionable legal opinions, or fabricated documents are used to make restricted shares appear eligible for public-market sale.
170. **Auction Indicative-Price Spoofing** — Large orders are entered during an opening or closing auction to move the theoretical auction price and then cancelled before execution.
171. **Auction Uncrossing-Volume Manipulation** — Orders are intentionally used to distort indicative auction volume or imbalance even when the manipulator does not genuinely intend to trade that quantity.
172. **Pre-Expiry Related-Instrument Manipulation** — A security is pushed up, down, or held at a particular level shortly before the issuance, redemption, or expiry of a related derivative or convertible.
173. **Strike / Barrier Pinning** — Trading keeps an underlying stock artificially above or below an option strike or derivative barrier so the related instrument produces a desired payout.
174. **Barrier-Trigger Manipulation** — The underlying security is deliberately pushed through, or prevented from reaching, a strike, knock-in, knock-out, or other derivative trigger.
175. **Margin-Settlement Reference Manipulation** — A settlement price used to calculate collateral or margin requirements is manipulated so another position receives a financial benefit.
176. **Artificial Arbitrage Creation** — The reference price of one security is manipulated specifically to manufacture or enlarge an arbitrage opportunity against a related security.
177. **Rights-Issue Reference-Price Manipulation** — Shares or rights are manipulated around theoretical prices so that an artificial arbitrage opportunity is created during a rights issue.
178. **Insider Order Amendment / Cancellation** — Someone who learns material non-public information uses it to cancel or modify an order already resting in the market.
179. **Inside-Information Recommendation / Inducement** — A person possessing inside information recommends or pressures someone else to buy, sell, amend, or cancel an order.
180. **Wall-Cross / Market-Sounding Insider Trading** — An investor receiving confidential deal information during a market sounding trades before the information becomes public.
181. **Shadow Trading** — A person uses confidential information about one company to trade securities of another economically linked or competing company expected to move from the same information.
182. **Cyber-Hacked MNPI Trading** — A fraudster hacks an issuer, law firm, or information holder, steals material non-public information, and trades before it becomes public.
183. **Fake Tender-Offer / Regulatory-Filing Manipulation** — A fraudulent takeover or similar filing is submitted through an official regulatory system to create apparently credible market-moving news.
184. **Rule 10b5-1 Trading-Plan Abuse** — An insider establishes or uses a supposedly pre-planned trading arrangement opportunistically while possessing material non-public information.
185. **Mutual-Fund Late Trading** — Mutual-fund transactions are submitted after the pricing cutoff while improperly receiving the earlier NAV.
186. **Deceptive Mutual-Fund Market Timing** — Rapid fund purchases and redemptions exploit stale pricing or time-zone differences while accounts or identities are structured to circumvent restrictions.
187. **Selective Portfolio-Holdings Abuse** — Confidential mutual-fund portfolio information is secretly provided to favored traders who use it for market timing or to trade against the fund.
188. **Issuer Share-Repurchase Manipulation** — A company uses its own-market purchases with manipulative intent to support or influence its stock price.
189. **Improper Price Stabilization** — Stabilization purchases connected with an offering are conducted outside permitted conditions and are used to artificially support the security.
190. **Liquidity-Rebate Wash Trading** — Wash or self-offsetting transactions are generated primarily to earn exchange liquidity rebates rather than for genuine economic trading.
191. **NBBO Join-Inducement / Disruptive Quoting** — A trader places a non-genuine order inside the best bid or offer, waits for another participant to join it, then trades against that participant from the opposite side.
192. **Toxic / Death-Spiral Convertible Manipulation** — A convertible holder deliberately pressures the underlying stock downward because the lower price gives the holder more shares upon conversion.
193. **Engineered Short Squeeze** — A participant deliberately manipulates the price or available supply of shares to force short sellers to cover at artificially high prices.
194. **Hacked-Account Forced-Buy Pump** — Compromised brokerage accounts are forced to buy thinly traded securities, driving up price and volume so fraudsters can sell at inflated prices.
195. **Account-Takeover Options Cross-Trade Fraud** — A hacked customer account is made to buy or sell illiquid options at unreasonable prices against a separate fraudster-controlled account.
196. **New-Account Identity Options Fraud** — A stolen or synthetic identity is used to create an account that deliberately enters uneconomic options trades against another fraudster-controlled account.
197. **Share-Journaling Fragmentation Scheme** — A large position is deposited and then journaled or transferred across numerous apparently unrelated accounts to conceal common control before liquidation.
198. **Nested Omnibus Concealment** — One financial intermediary trades through another intermediary’s omnibus account so the real underlying investors and coordinated activity become difficult to identify.
199. **Imposter Market-Information Source Manipulation** — Fraudsters impersonate a company, analyst, regulator, or trusted source and disseminate false market-moving information.
200. **Distribution Restricted-Period Manipulation** — An issuer, underwriter, or distribution participant improperly bids for or purchases a security during a restricted offering period to influence market price.
201. **ETP Creation/Redemption Exploitation** — The creation or redemption mechanics of an ETF or other ETP are exploited together with market trading to produce an artificial economic advantage.
202. **ETP Portfolio-Composition Manipulation** — A trader exploits known or anticipated changes to an ETP’s underlying portfolio composition to manipulate the ETP or component securities.
203. **Correlated ETP / Options Manipulation** — Positions in correlated ETFs, index options, or related products are used together so manipulation in one product produces profitable changes in another.
204. **ACATS Account-Transfer Fraud** — A fraudster uses a victim’s identity to create a brokerage account, fraudulently transfers the victim’s securities, and then attempts to liquidate or move the stolen assets.
205. **Digital-Signature Trading-Authorization Forgery** — Customer or supervisor signatures are forged on trading authorizations, transaction approvals, wire instructions, or other brokerage documents.
206. **ACH Instant-Funds Securities Abuse** — A fraudulent account exploits immediate buying power granted before an ACH deposit fully settles and trades with effectively unfunded money.
207. **Direct-To-Investor Stock-Manipulation Scam** — Scammers use misdirected texts, private investment groups, or similar direct communication to convince victims to buy a targeted thinly traded stock.
208. **Fictitious Quotation** — A broker or trader publishes a bid or offer that is not a bona fide expression of actual trading interest.
209. **False Transaction Publication** — A participant reports or circulates a purported securities transaction even though the reported purchase or sale was not genuine.
210. **Closing-Bid Marking** — Orders or quotation changes near the end of the session artificially raise or maintain the best closing bid even when little or no genuine trading occurs there.
211. **False Appearance of Customer Interest** — Orders are entered so other market participants believe genuine customer demand exists when the supposed interest was created primarily to influence price.
212. **Quote Coordination Between Market Participants** — Competing traders or firms secretly coordinate quotations or prices instead of allowing independent competitive price formation.
213. **Coordinated Trade-Reporting Manipulation** — Multiple firms coordinate how or when transactions are reported so published market activity creates an artificial or misleading impression.
214. **Pressure-to-Alter-Quote Scheme** — One market participant pressures or directs another market maker to raise, lower, or maintain its quotation for an improper purpose.
215. **Market-Maker Retaliation / Intimidation** — A participant threatens or economically pressures another market maker to discourage competitive quotations or trading behavior.
216. **Paid Market-Making Influence** — An issuer, promoter, or affiliate improperly compensates a broker-dealer for publishing quotations or making a market in the issuer’s security.
217. **Paid Publication to Influence Stock Price** — A broker or associated party pays a person, publication, website, or media outlet to publish material intended to influence a security’s market price.
218. **Non-Firm Stated-Price Offer** — A broker advertises an offer to buy or sell at a stated price even though it is not genuinely prepared to transact under those conditions.
219. **Broker Inventory Parking to Evade Capital Requirements** — Securities are temporarily moved to another account or firm around a reporting date to hide inventory and artificially improve regulatory capital.
220. **Mark-to-Market Inflation** — A broker or manager intentionally assigns inflated marks to positions so reported capital, profits, NAV, or portfolio values appear higher than actual economic value.
221. **Investment-Fund Asset Overvaluation** — A fund adviser deliberately overvalues difficult-to-price securities, inflating reported performance or NAV.
222. **Performance-Fee Inflation Through False Valuation** — Portfolio assets are intentionally marked too high so an adviser can report exaggerated returns or collect larger management/performance fees.
223. **Fraudulent NAV Pricing** — An investment fund intentionally uses false asset prices when calculating NAV, causing investors to transact at incorrect values.
224. **Unfair Client Cross-Trade Pricing** — A broker crosses a security between two customer accounts at a price intentionally favorable to one and unfair to the other.
225. **Personal-Account Cross-Trade Abuse** — A broker or adviser secretly trades between a customer’s account and their own account at manipulated or unfair prices.
226. **Riskless-Principal Compensation Concealment** — A broker executes an offsetting principal trade and fails to disclose the markup, markdown, or economic compensation earned.
227. **Error-Account Abuse** — A firm’s error account is intentionally used for transactions that are not genuine trading errors, such as concealing positions, shifting losses, or providing unauthorized benefits.
228. **Margin-Evasion Through Error Accounts** — Customer transactions are deliberately moved into a firm’s error account so the customer or firm can avoid margin requirements.
229. **Cancel-and-Rebill Manipulation** — An executed trade is improperly cancelled and rebooked with altered account, price, capacity, or other information to shift gains, losses, or regulatory consequences.
230. **Selling Away** — A registered representative arranges securities transactions outside their brokerage firm without the firm’s required knowledge or approval.
231. **Piggybacking / Shadowing Customer Trades** — A broker uses knowledge of customer trading to place similar personal or favored-account trades for its own benefit.
232. **Sham Married-Put Short-Sale Evasion** — Stock and deep-in-the-money puts are combined in a transaction lacking genuine economic substance to disguise what is economically a short position.
233. **Buy-Write Fail-to-Deliver Reset Scheme** — A trader repeatedly combines stock purchases with deep-in-the-money call sales to create the appearance that a failure-to-deliver has been closed while it effectively continues.
234. **Stock-Kiting to Maintain Naked Short Position** — Repeated matched or reset transactions are used to make it appear that securities are being delivered while shares continually circulate and the fail remains outstanding.
235. **Stock-Loan Sham Finder-Fee Scheme** — Securities-lending transactions include payments to supposed stock-loan finders who performed no genuine service.
236. **Stock-Loan Kickback Scheme** — Securities-lending traders arrange inflated or sham finder fees and secretly receive part of those payments back as personal kickbacks.
237. **Neighboring Options-Series Spoofing** — Fake orders are placed across several adjacent strike/expiration series to manipulate option prices while genuine orders execute at the resulting artificial prices.
238. **Trade Shredding** — A customer order or execution is deliberately split into many smaller trades primarily to maximize market-data payments, rebates, or similar compensation.
239. **IPO / New-Issue Withholding for Firm or Insider Benefit** — Attractive new-issue shares are withheld from genuine public customers so the broker, its personnel, or favored insiders can obtain the allocation.
240. **Restricted-Person New-Issue Allocation** — IPO shares are improperly allocated to broker-dealer insiders or other restricted persons who should not benefit from privileged access.
241. **IPO Flipping-Penalty Recoupment Abuse** — A firm improperly takes back an employee’s commission or applies another penalty because a customer quickly resold IPO shares.
242. **Undisclosed IPO Lock-Up Release / Waiver** — An underwriter releases insiders from an IPO lock-up without required advance public disclosure.
243. **Pre-Opening IPO Market-Order Abuse** — Market orders for newly issued shares are accepted before secondary trading begins, exposing investors to unpredictable executions and potentially distorting opening-price formation.
244. **Returned IPO Share Premium Capture** — Shares returned to an underwriting syndicate after an IPO are improperly resold at a premium for the firm’s benefit.
245. **Payment-for-Order-Flow Biased Routing** — Customer orders are routed toward the venue paying the broker the largest PFOF rather than the venue offering customers the best available execution.
246. **Maker-Taker Rebate Biased Routing** — Exchange rebates or maker-taker economics improperly influence where customer orders are routed, potentially sacrificing execution quality.
247. **Affiliated-Venue Routing Conflict** — A broker preferentially sends customer orders to an affiliated ATS, market maker, or exchange because the firm benefits economically from the affiliate.
248. **Undisclosed Principal Trading with Advisory Clients** — An adviser trades securities directly with a client for its own account without required transaction-specific disclosure and consent.
249. **Improper Agency-Cross Transaction** — An adviser represents both buyer and seller in the same transaction without satisfying required disclosure, consent, and conflict protections.
250. **Wrap-Fee Trading-Away Cost Concealment** — A supposedly all-inclusive wrap account executes substantial trades through outside brokers, generating extra charges that clients were not adequately told they would pay.
251. **Fully Paid Securities Lending Without Proper Consent or Disclosure** — A broker places customers’ fully paid shares into a securities-lending program without satisfying required consent or disclosure requirements.
252. **Unsuitable Fully Paid Securities Lending** — A broker recommends lending a customer’s fully paid securities without adequately determining whether participation is appropriate.
253. **Improper Short-Exempt Marking** — A normal short sale is incorrectly marked `short exempt` so that it receives treatment reserved for qualifying exemptions.
254. **Short-Sale Circuit-Breaker Exemption Abuse** — A participant improperly invokes an exemption to continue short selling after a price-test circuit breaker has restricted ordinary short sales.
255. **Stop-Loss Trigger Manipulation** — A participant intentionally moves the market toward known or anticipated stop levels so stop-loss orders activate and create additional forced trading.
256. **Tender-Offer Outside-Purchase / Unequal-Consideration Abuse** — A bidder or covered participant purchases target shares outside the tender offer on better or different terms than those available to tendering investors.
257. **Short Tendering** — A participant tenders more securities into a partial tender offer than its actual net-long position.
258. **Hedged Tendering** — A participant tenders shares and then sells or hedges the same economic position so effectively more shares are exposed to the tender than genuinely owned.
259. **Improper ADR Pre-Release** — ADRs are pre-released without properly ensuring that the broker or customer owns the corresponding foreign shares represented by those ADRs.
260. **Interfund Cross-Trade Conflict Abuse** — An adviser crosses securities between two funds or managed accounts where conflicting duties cause one client to receive an unfair benefit at another’s expense.
261. **Interfund Cross-Trade Mispricing** — Securities are transferred between related funds or accounts at prices that deviate from independent current market value, shifting value between clients.
262. **Derivative-Based Beneficial-Ownership Evasion** — Swaps or other derivatives are deliberately structured to obtain economic exposure or influence while preventing ownership from appearing in required disclosures.
263. **Intentional Schedule 13D Ownership Concealment** — A person deliberately fails to disclose a reportable beneficial-ownership position so the market does not know who controls or is accumulating the stock.
264. **Hidden Beneficial-Ownership Group** — Multiple investors coordinate acquisition, holding, voting, or disposition of shares while presenting themselves as independent investors to avoid group-level disclosure.
265. **Late Beneficial-Ownership Filing After Threshold Crossing** — An investor exceeds a disclosure threshold but fails to make the required ownership filing within the applicable period.
266. **Manufactured Credit Event** — A security-based-swap participant deliberately causes or influences an artificial credit event so a credit derivative produces a desired payout.
267. **Security-Based-Swap Payment/Delivery Manipulation** — Conduct is engineered to improperly alter payments, deliveries, or continuing obligations under a security-based swap.
268. **Security-Based-Swap Valuation Manipulation** — A participant manipulates information, underlying markets, or other inputs specifically to distort the price or valuation of a security-based swap.
269. **Retail Execution-Quality Misrepresentation** — A broker tells customers their orders receive superior liquidity or price improvement while its actual execution process provides materially worse treatment.
270. **Internalization Profit at Customer Expense** — An order-routing process deliberately gives customers inferior execution while enabling the broker to internalize portions of their orders for its own benefit.
271. **Issuer Buyback While Possessing MNPI** — A company repurchases its own shares while possessing favorable material non-public information.
272. **Dark-Pool Subscriber Information Trading Abuse** — A dark-pool operator or affiliate uses confidential subscriber order information to trade proprietarily against or ahead of those subscribers.
273. **Dark-Pool Subscriber Information Marketing Abuse** — Confidential customer trading information is improperly used to market an ATS or demonstrate supposed liquidity to prospective clients.
274. **Secret Proprietary Desk Inside an ATS** — An ATS operator secretly runs a proprietary trading operation with access to or advantages from customer activity without adequately disclosing the conflict.
275. **Hidden Affiliate Counterparty Dominance** — A dark pool claims to match natural independent buyers and sellers while an affiliated trading operation is actually the counterparty to most executions.
276. **False Dark-Pool Anti-Predatory-Trading Claims** — A venue advertises protection from arbitrageurs or HFTs while those traders or affiliated strategies are actually present or advantaged.
277. **Undisclosed Preferential ATS Order Type** — A trading venue gives certain participants access to an advantageous order type without adequately disclosing that functionality to all subscribers.
278. **Sub-Penny Priority Jumping** — An ATS accepts and ranks sub-penny orders that allow privileged participants to step ahead of lawful whole-penny orders.
279. **Selective ATS Functionality Access** — Certain customers receive trading functionality, settings, or execution capabilities unavailable to other subscribers without adequate disclosure.
280. **Execution-Venue Masking** — A broker changes trade reports, invoices, or customer reports so executions at outside firms appear to have occurred at the broker itself.
281. **Selective Early Market-Data Dissemination** — An exchange gives proprietary-data customers trading information before transmitting the same information to the consolidated or public feed.
282. **Selective Advance Distribution of Research** — A research report or recommendation is provided to selected trading personnel or favored customers before other customers entitled to receive it.
283. **Analyst Trading Against Own Recommendation** — A research analyst or controlled account trades a covered security in a manner inconsistent with the analyst’s published recommendation or applicable restrictions.
284. **Research-Analyst Blackout Trading** — An analyst trades the covered security during a prohibited blackout period surrounding publication or modification of research.
285. **Research-Analyst Selective MNPI Disclosure** — An analyst selectively provides material non-public information to traders, customers, or other favored parties instead of maintaining proper information barriers.
286. **False Securities-Loan Reporting** — A securities borrower, lender, or reporting intermediary submits fictitious or deliberately false securities-loan information to the regulatory reporting system.
287. **Securities-Loan Quantity Misreporting** — The number of shares or par amount actually borrowed or lent is deliberately misstated in securities-lending reports.
288. **Securities-Loan Rate / Fee Misreporting** — Lending fees, rebate rates, spreads, or reference-rate information are deliberately reported incorrectly to conceal the true economics of a securities loan.
289. **Securities-Loan Modification Concealment** — Material modifications, terminations, or changes to an existing securities loan are intentionally not reported or are falsely reported.
290. **Undercollateralized Customer Securities Borrowing** — A broker borrows fully paid or excess-margin customer securities without providing collateral sufficient to fully secure them.
291. **Customer Fully-Paid Securities Possession/Control Abuse** — A broker improperly uses, pledges, or fails to obtain possession or control of customer fully paid or excess-margin securities.
292. **Customer Reserve Requirement Manipulation Through Affiliates** — A broker structures transactions with affiliated entities to artificially reduce the amount of customer cash it must maintain in protected reserve accounts.
293. **Inside-Spread Spoofing** — Placing a non-genuine order inside the spread to alter the best bid or offer, then cancelling it after other participants react.
294. **Best-Bid Spoofing** — Displaying a deceptive bid at or near the best bid to create artificial buying pressure while seeking to trade elsewhere or on the opposite side.
295. **Best-Offer Spoofing** — Displaying a deceptive offer at or near the best offer to create artificial selling pressure while seeking to trade elsewhere or on the opposite side.
296. **Away-From-Touch Spoofing** — Placing large non-genuine orders several price levels from the market to alter visible depth and influence other traders.
297. **Cancel-on-Touch Spoofing** — Repeatedly cancelling a displayed order as soon as execution becomes likely, consistent with an intent to influence rather than trade.
298. **Replenishing Spoof Orders** — Repeatedly replacing cancelled deceptive orders so false displayed pressure remains visible as the market moves.
299. **Moving-Wall Manipulation** — Moving a large deceptive bid or offer wall with the market so the apparent support or resistance follows the price.
300. **Fake Support Wall** — Displaying large non-genuine bids intended to create an artificial impression of strong price support.
301. **Fake Resistance Wall** — Displaying large non-genuine offers intended to create an artificial impression of strong selling resistance.
302. **Synthetic Depth Creation** — Using many related orders to manufacture the appearance of deep liquidity that is not genuinely available.
303. **Depth Withdrawal Manipulation** — Rapidly withdrawing a large share of displayed liquidity in a coordinated manner to create an artificial liquidity shock.
304. **Liquidity-Vacuum Manipulation** — Creating and then abruptly removing displayed liquidity to provoke price movement or forced trading by others.
305. **Quote-Fade Manipulation** — Displaying attractive liquidity and systematically withdrawing it when other participants attempt to trade against it.
306. **Bait-and-Switch Quoting** — Showing an attractive quote to induce reactions and then replacing it with materially worse terms before execution.
307. **Queue-Position Manipulation** — Using repeated order entry, cancellation, or modification primarily to gain or distort execution priority rather than express genuine trading interest.
308. **Queue-Depletion Manipulation** — Submitting aggressive or self-related activity to remove orders ahead in the queue and improve a favored order's execution position.
309. **Order-Priority Gaming with Related Accounts** — Coordinating related accounts so one account changes queue conditions to benefit another account's resting order.
310. **Minimum-Quantity Probing Abuse** — Using repeated minimum-quantity orders primarily to discover hidden liquidity and exploit the discovered trading interest.
311. **IOC Liquidity-Probing Abuse** — Using repeated immediate-or-cancel orders to map hidden liquidity or another participant's strategy and then exploit it manipulatively.
312. **FOK Liquidity-Probing Abuse** — Using repeated fill-or-kill orders to test hidden size or liquidity conditions before executing a manipulative strategy.
313. **Odd-Lot Quote Shaping** — Using a series of odd-lot orders to influence displayed price signals, reference calculations, or other participants' algorithms.
314. **Odd-Lot Last-Sale Marking** — Using very small executions to establish misleading last-sale prices or price trends.
315. **Micro-Order Spoofing** — Using many very small non-genuine orders to create false order-book activity while minimizing execution risk.
316. **Microburst Spoofing** — Submitting and cancelling deceptive orders in extremely short bursts to influence algorithms or visible depth.
317. **Cross-Symbol Quote Stuffing** — Flooding order traffic across multiple related securities to distort market-data processing or hide manipulative activity.
318. **Feed-Delay Quote Stuffing** — Generating excessive order messages with the purpose of creating data-processing delays that can be exploited.
319. **Locked-Market Inducement** — Using deceptive orders to create or encourage locked quotations for manipulative advantage.
320. **Crossed-Market Inducement** — Using deceptive orders to create or encourage crossed quotations for manipulative advantage.
321. **Spread-Widening Manipulation** — Withdrawing or placing orders strategically to make the quoted spread artificially wider for economic advantage.
322. **Spread-Narrowing Manipulation** — Placing non-genuine quotes to make the spread appear artificially tight and induce other participants to trade.
323. **One-Sided Book Pressure Manipulation** — Concentrating deceptive orders on one side of the book to create a false directional signal.
324. **Book-Pressure Flip Manipulation** — Rapidly switching deceptive depth from one side of the book to the other to trigger or exploit participant reactions.
325. **Order-Ladder Manipulation** — Creating a deceptive staircase of orders with systematically increasing or decreasing prices or sizes to suggest directional conviction.
326. **Reserve-Order Replenishment Manipulation** — Using hidden or reserve orders together with deceptive displayed orders to mislead participants about genuine available liquidity.
327. **Iceberg-Spoof Combination** — Combining hidden genuine liquidity with large deceptive displayed orders so the visible book misrepresents the trader's true objective.
328. **Midpoint-Peg Manipulation** — Using orders or trades to move the reference quote so midpoint-pegged orders execute at an artificially favorable level.
329. **Pegged-Order Reference Manipulation** — Manipulating the reference market used by pegged orders so those orders reprice in a way that benefits the manipulator.
330. **Last-Sale Price Marking** — Executing trades primarily to move or maintain the published last-sale price at an artificial level.
331. **Intraday High Marking** — Executing a small or strategically timed trade to establish an artificial session high.
332. **Intraday Low Marking** — Executing a small or strategically timed trade to establish an artificial session low.
333. **Microtrade Price Setting** — Using very small trades in an illiquid security to set or move the observable market price.
334. **Sequence Painting** — Executing a planned sequence of trades that creates a false visual impression of a rising, falling, or active market.
335. **Alternating Print Manipulation** — Alternating buys and sells among related accounts to manufacture a misleading pattern of price movement and activity.
336. **Volume-Pulse Manipulation** — Concentrating coordinated trades in short bursts to create false indications of sudden market interest.
337. **Trade-Cluster Momentum Manipulation** — Clustering transactions closely in time to imitate organic momentum and induce follow-on trading.
338. **Quote-and-Trade Combination Manipulation** — Coordinating deceptive quotes with genuine trades so the quotes move the market and the trades capture the benefit.
339. **Uneconomic Value-Transfer Trading** — Executing intentionally unfavorable trades between related accounts to transfer money or profit through securities prices.
340. **Profit-Transfer Matched Trading** — Using coordinated matched trades at selected prices to shift profits from one account to another.
341. **Loss-Transfer Matched Trading** — Using coordinated matched trades at selected prices to shift losses from one account to another.
342. **Client-to-Proprietary Value Transfer** — Crossing or arranging trades at unfair prices so value is shifted from a customer account to a proprietary account.
343. **Proprietary-to-Favored-Client Value Transfer** — Using intentionally favorable executions to transfer value from a firm or other account to a favored customer.
344. **Multi-Venue Wash Trading** — Executing economically self-offsetting trades across different venues to hide common control and manufacture volume.
345. **Cross-Product Wash Trading** — Using economically offsetting trades in related products to create artificial activity or transfer value.
346. **Affiliate Wash Trading** — Executing wash trades between separately named affiliates under common control.
347. **Omnibus Wash Trading** — Using omnibus or pooled accounts to conceal self-trading or common beneficial ownership.
348. **Split-Quantity Matched Trading** — Dividing a prearranged trade into smaller matched executions to evade simple size-based surveillance.
349. **Staggered Matched Trading** — Separating coordinated matching trades in time to disguise a prearranged relationship.
350. **Intermediated Matched Trading** — Using one or more intermediary accounts between colluding parties to conceal matched or value-transfer trading.
351. **Variable-Quantity Circular Trading** — Passing a security through related accounts with varying quantities to disguise a circular trading loop.
352. **Cross-Venue Circular Trading** — Executing legs of a circular trading scheme on different venues to prevent any venue from seeing the entire cycle.
353. **Synthetic Round-Trip Trading** — Using multiple securities or derivatives to create an economically circular position that manufactures activity without meaningful risk transfer.
354. **Reverse Round-Trip Trading** — Executing a sequence that transfers a position away and then rapidly returns it to the original economic owner at engineered prices.
355. **Closing-Print Wash Trading** — Using wash or self-related trades near the close to set or support the closing price.
356. **Opening-Print Wash Trading** — Using wash or self-related trades near the open to set or influence the opening price.
357. **Benchmark-Window Wash Trading** — Executing wash or prearranged trades during a benchmark calculation window to influence the benchmark value.
358. **VWAP-Window Concentration Manipulation** — Concentrating manipulative executions during high-weight portions of a VWAP calculation window.
359. **TWAP-Window Concentration Manipulation** — Concentrating manipulative executions at selected intervals to distort a time-weighted benchmark.
360. **Off-Market Cross Price Marking** — Executing a cross at an artificial price to create a misleading reference or last-sale price.
361. **Negotiated-Cross Price Marking** — Using a pre-negotiated cross primarily to influence published price rather than transfer genuine market risk.
362. **Banging the Close** — Aggressively buying or selling near the end of trading to force the closing price in a desired direction.
363. **Banging the Open** — Aggressively buying or selling around the opening process to force the opening price in a desired direction.
364. **End-of-Month Marking** — Trading near month-end to influence reported portfolio values, benchmarks, or performance.
365. **Quarter-End Marking** — Trading near quarter-end to influence valuations, performance, collateral, or reported positions.
366. **Year-End Marking** — Trading near year-end to influence valuations, performance, tax positions, or reported market prices.
367. **Index-Rebalance Close Manipulation** — Trading constituent securities near a rebalance close to distort index-related execution or settlement prices.
368. **ETF-Rebalance Close Manipulation** — Trading around an ETF rebalance close to influence component or fund execution prices.
369. **Pre-Close Ramp-and-Reverse** — Driving a price shortly before the close and reversing the position after the closing price has been established.
370. **Closing-Auction Order Flooding** — Submitting a large burst of auction orders to distort imbalance, indicative volume, or participant behavior.
371. **Opening-Auction Order Flooding** — Submitting a large burst of auction orders to distort opening imbalance, indicative volume, or participant behavior.
372. **Late Auction Cancellation Abuse** — Entering large auction interest and cancelling it late in the auction process after influencing indicative prices or imbalance.
373. **Auction-Extension Triggering Manipulation** — Submitting orders intended to force an auction extension or volatility interruption for strategic benefit.
374. **Auction-Extension Avoidance Manipulation** — Withdrawing or modifying orders to avoid a price extension or auction condition that would otherwise occur.
375. **Auction-Price-Collar Gaming** — Structuring orders around auction price collars or protection bands to manipulate the resulting uncrossing price.
376. **Closing-Cross Participation Manipulation** — Using coordinated accounts or orders to dominate a closing cross and influence the official closing price.
377. **Opening-Cross Participation Manipulation** — Using coordinated accounts or orders to dominate an opening cross and influence the official opening price.
378. **Settlement-Window Ramping** — Aggressively trading during a settlement-price window to push the official settlement value.
379. **Settlement-Window Wash Trading** — Using wash or related-party trades during a settlement window to influence the calculated settlement price.
380. **Fixing-Window Manipulation** — Concentrating orders or trades during an official fixing window to distort the fix.
381. **Benchmark-Window Concentration** — Concentrating an unusually large share of trading in a benchmark window to influence the resulting benchmark.
382. **Index-Calculation Constituent Marking** — Manipulating one or more constituent prices at calculation time to influence an index level.
383. **NAV-Strike Price Manipulation** — Manipulating underlying prices near a fund or derivative NAV strike time to affect valuation or settlement.
384. **Stock-to-Option Marking** — Trading the underlying stock to move option values or facilitate favorable option executions.
385. **Option-to-Stock Signaling Manipulation** — Using option orders or trades to create signals intended to influence trading in the underlying stock.
386. **ETF-to-Component Manipulation** — Trading an ETF to influence prices or behavior in one or more underlying component securities.
387. **Component-to-ETF Manipulation** — Manipulating constituent securities to change an ETF's price, NAV, premium, or discount.
388. **ADR-to-Local-Share Manipulation** — Trading ADRs to influence the related locally listed shares or vice versa.
389. **Dual-Listed Share Manipulation** — Using activity in one listing of a security to manipulate the price or execution in another listing.
390. **Futures-to-Cash Manipulation** — Trading a related future to influence the cash security or basket price.
391. **Cash-to-Futures Settlement Manipulation** — Trading cash securities to influence the settlement or value of related futures.
392. **Convertible-to-Underlying Manipulation** — Using convertible securities activity to influence the underlying equity or exploiting manipulated equity prices to benefit the convertible.
393. **Warrant-to-Underlying Manipulation** — Using warrants and the underlying stock together so manipulated prices in one produce gains in the other.
394. **Rights-to-Underlying Manipulation** — Trading rights and ordinary shares together to manipulate relative pricing or exercise economics.
395. **Preferred-to-Common Manipulation** — Manipulating common or preferred shares to distort relative value between related capital-structure instruments.
396. **Correlated-Basket Spoofing** — Spoofing a basket of correlated securities to influence another instrument or basket in which the trader holds a genuine position.
397. **Lit-to-Dark Manipulation** — Using deceptive activity on a lit market to influence executions in a dark pool that references lit prices.
398. **Dark-to-Lit Manipulation** — Using activity or information from dark trading to manipulate prices or liquidity on lit venues.
399. **Dark-Pool Reference-Price Manipulation** — Moving the external reference price used by a dark pool so midpoint or reference-priced executions occur at artificial levels.
400. **Auction-to-Continuous Manipulation** — Using auction orders to influence prices or participant behavior in continuous trading before or after the auction.
401. **Continuous-to-Auction Manipulation** — Using continuous-market trading to distort the indicative or final auction price.
402. **Cross-Border Price-Lead Manipulation** — Manipulating the market that normally leads price discovery to influence the same or related security in another jurisdiction.
403. **Pair-Trade Manipulation** — Manipulating one leg of a pair or relative-value trade to create or enlarge profit on the other leg.
404. **Gamma-Squeeze Manipulation Scheme** — Coordinating option and stock trading with manipulative intent to force hedging flows that amplify the underlying price move.
405. **Creation-Basket Manipulation** — Manipulating securities included in an ETP creation basket to obtain an artificial advantage in creation transactions.
406. **Redemption-Basket Manipulation** — Manipulating securities included in an ETP redemption basket to obtain an artificial advantage in redemption transactions.
407. **Rebalance-Price-Impact Exaggeration** — Trading around an index or fund rebalance with the intent to amplify predictable price pressure and profit from the distortion.
408. **Front Running Customer Limit Orders** — Trading ahead after learning of a customer's significant limit order likely to affect price or liquidity.
409. **Front Running Stop Orders** — Trading ahead after learning the location or size of customer stop orders likely to trigger market movement.
410. **Front Running Market-on-Close Orders** — Trading ahead of known customer market-on-close flow expected to affect the closing auction or price.
411. **Front Running VWAP Orders** — Trading ahead of a known customer VWAP program to profit from its expected execution pattern.
412. **Front Running Algorithmic Parent Orders** — Inferring or learning a large parent order and trading ahead of its expected child-order flow.
413. **Front Running Iceberg Orders** — Detecting or learning a large hidden order and trading ahead of subsequent replenishment or execution.
414. **RFQ Front Running** — Trading before or around a customer request-for-quote using confidential knowledge of the customer's intended transaction.
415. **RFQ Information Leakage Abuse** — Sharing or exploiting confidential RFQ information so another party can trade ahead or adjust prices unfairly.
416. **Abusive Pre-Hedging** — Trading before a client transaction in a manner that exceeds legitimate risk management and disadvantages the client.
417. **Front Running Block Crosses** — Trading ahead of a planned block cross after receiving confidential information about the transaction.
418. **Front Running Offerings** — Trading ahead of a confidential equity or debt offering using knowledge of expected issuance or hedging flows.
419. **Front Running Tender Activity** — Trading ahead of known tender, repurchase, or exchange activity using confidential order information.
420. **Front Running Benchmark Fixes** — Trading ahead of known benchmark-fixing orders to profit from the expected price impact.
421. **Trading with Knowledge of Auction Imbalance** — Using non-public or improperly obtained auction imbalance information to trade before the market incorporates it.
422. **Trading with Knowledge of Order Imbalance** — Using confidential information about a significant order imbalance to trade for personal or favored-account benefit.
423. **Misuse of Customer IOI Information** — Using a customer's confidential indication of interest to trade ahead, adjust quotes, or disadvantage the customer.
424. **Misuse of Portfolio-Composition Files** — Using confidential or prematurely obtained fund portfolio data to trade ahead of expected creations, redemptions, or rebalances.
425. **Misuse of Stop-Level Information** — Using confidential customer stop-order information to trade toward, trigger, or profit from those stop levels.
426. **Misuse of Dark-Pool Order Information** — Using confidential dark-pool order or subscriber information for proprietary or favored-account trading.
427. **Insider Trading Before Earnings** — Trading while possessing material non-public earnings or financial-results information before public release.
428. **Insider Trading Before Dividend Action** — Trading while possessing non-public information about a dividend initiation, increase, cut, or suspension.
429. **Insider Trading Before Buyback Announcement** — Trading while possessing material non-public information about an issuer repurchase program.
430. **Insider Trading Before Capital Raise** — Trading while possessing material non-public information about an upcoming equity or debt financing.
431. **Insider Trading Before Bankruptcy or Restructuring** — Trading while possessing non-public information about imminent insolvency, bankruptcy, restructuring, or rescue financing.
432. **Insider Trading Before Rating Action** — Trading while possessing confidential information about a material credit-rating change or similar event.
433. **Long-Sale Mismarking** — Intentionally marking a sale as long when the seller does not have the required ownership or delivery position.
434. **Sham Locate** — Using a false or non-bona-fide locate representation to satisfy short-sale requirements without genuine borrow availability.
435. **Locate Reuse Abuse** — Improperly reusing the same locate or borrow availability for multiple short sales beyond what is genuinely available.
436. **Hard-to-Borrow Locate Fraud** — Misrepresenting the availability of scarce hard-to-borrow shares to facilitate short selling.
437. **Synthetic-Short Concealment** — Using options, swaps, or other instruments to conceal an economically short position from applicable surveillance or disclosure.
438. **Fail-to-Deliver Concealment** — Using transactions or reporting practices to hide persistent failures to deliver securities.
439. **Sham Close-Out Transaction** — Executing a transaction with little or no economic substance solely to create the appearance that a close-out obligation was satisfied.
440. **Threshold-Security Close-Out Evasion** — Structuring trades to avoid mandatory close-out requirements for persistent settlement failures.
441. **Chronic Fail Manipulation** — Maintaining persistent failures to deliver as part of a broader manipulative strategy.
442. **Stock-Borrow Squeeze Manipulation** — Acquiring or withholding lendable shares to create artificial scarcity and pressure short sellers.
443. **Borrow-Fee Manipulation** — Coordinating securities-lending activity to distort borrow fees or rebate rates for economic advantage.
444. **Securities-Lending Wash Transaction** — Arranging offsetting stock-loan transactions with no genuine economic purpose to manufacture activity, fees, or regulatory appearances.
445. **Matched Stock-Loan Transactions** — Prearranging securities loans between related parties to transfer value or disguise ownership and availability.
446. **Recall-Timing Manipulation** — Coordinating the recall of lent securities to create artificial scarcity or price pressure at a strategically important time.
447. **Locate-Availability False Signaling** — Publishing or communicating misleading information about borrow availability to influence short sellers or security prices.
448. **Borrow-Inventory Corner** — Obtaining control of a large portion of lendable inventory to manipulate stock-borrow conditions or force unfavorable covering.
449. **Short-Squeeze Ignition** — Using manipulative buying, liquidity withdrawal, or messaging to deliberately trigger forced covering by short sellers.
450. **Ex-Clearing Settlement Evasion** — Using ex-clearing or bilateral settlement arrangements to conceal settlement failures or evade standard close-out controls.
451. **Accumulation-Promotion-Distribution Scheme** — Coordinated accounts accumulate a position, create or support promotional demand, and then distribute shares into the inflated market.
452. **Coordinated Limit-Order Pump** — Victims or related accounts are directed to place coordinated limit orders that raise the price while controlling accounts sell into the demand.
453. **Opening-Gap Pump** — Concentrated pre-open or opening-auction buying is used to create an artificial gap higher before shares are distributed.
454. **Closing-Price Pump** — Manipulative buying near the close is used to print a higher closing price that supports promotion or next-session selling.
455. **Low-Float Squeeze Pump** — A manipulator exploits limited public float to force a sharp price increase before distributing a concentrated position.
456. **Nominee-Account Pump** — Nominee accounts are coordinated to create apparently independent demand during a pump-and-dump scheme.
457. **Pump-and-Dump with Options** — Manipulative promotion and stock trading are combined with option positions that profit from the artificial price movement.
458. **Pump-and-Dump with Warrants** — Manipulative promotion and stock trading are used to increase the value or exercise economics of related warrants.
459. **Pump Before Financing** — A stock is manipulated upward before an equity financing so securities can be issued or sold on more favorable terms.
460. **Pump Before Insider Sale** — A stock is manipulated upward before a controlling holder or insider sells a significant position.
461. **Pump Before Share Issuance** — Price and volume are artificially supported before new shares, conversions, or exercises increase the public supply.
462. **Multi-Broker Dumping** — A concentrated position is distributed through several brokers to hide common control and reduce the visibility of the liquidation.
463. **Pump-and-Reload Cycle** — Manipulators repeatedly accumulate, pump, sell, allow the price to fall, and then repeat the scheme in the same security.
464. **Investment-Group Signal Scam Trading** — A coordinated group directs victims when and at what price to buy a targeted security while affiliated accounts sell into that demand.
465. **Nominee-Funnel-to-Omnibus Scheme** — Nominee accounts funnel shares into one or more omnibus accounts before a coordinated manipulation or liquidation.
466. **Victim-Order Price Support** — Fraudsters direct victims to place orders at specified prices so victim demand supports the stock while controlling accounts exit.
467. **Promotion-Triggered Liquidity Exit** — Promotional activity is timed specifically to create enough market liquidity for a concentrated holder to liquidate an otherwise difficult position.
468. **Rebate-Farming Self-Match** — Generating self-matches or wash-like trades primarily to earn maker rebates or other venue incentives.
469. **Fee-Tier Volume Inflation** — Creating artificial trading volume to qualify for lower fees, higher rebates, or preferred exchange pricing tiers.
470. **Market-Share Incentive Wash Trading** — Using non-economic trading to inflate reported market share and earn volume-based incentives.
471. **Liquidity-Program Gaming** — Creating artificial qualifying activity to obtain designated-market-maker or liquidity-provider benefits.
472. **Coordinated Quote Widening** — Multiple related or colluding participants widen quotes together to increase spreads or disadvantage incoming orders.
473. **Coordinated Quote Narrowing** — Multiple related or colluding participants display artificially tight quotes to induce flow or manipulate reference prices.
474. **Market-Maker Quote Synchronization** — Competing market makers coordinate quotation changes instead of independently setting prices.
475. **Fake Two-Sided Market** — Displaying both bid and offer interest that is not bona fide to create the appearance of a healthy or liquid market.
476. **Skewed Two-Sided Quote Manipulation** — Maintaining nominal two-sided quotes while using size and price asymmetry to create a deceptive directional signal.
477. **Conditional-Order Information Abuse** — Using confidential information from conditional orders to trade ahead or disadvantage the submitting participant.
478. **IOI Leakage Trading Abuse** — Leaking or exploiting confidential indications of interest to enable trading ahead or adverse price changes.
479. **Venue-Operator Proprietary Front Running** — A venue operator or affiliate uses knowledge of customer orders to trade proprietarily ahead of them.
480. **Selective Latency Advantage Abuse** — Providing or exploiting undisclosed preferential latency in a way that enables favored participants to trade against others unfairly.
481. **Hidden Order-Priority Advantage** — Giving selected participants undisclosed queue or matching priority over other orders.
482. **Broker Crossing-Engine Favoritism** — Operating a crossing system so a proprietary or favored counterparty receives systematically advantageous matches.
483. **Affiliate Internalization Favoritism** — Preferentially matching customer flow against an affiliate or proprietary account despite conflicts or inferior outcomes.
484. **Internalizer Price-Shading Abuse** — Adjusting internal execution prices to capture value from customers while presenting the executions as fair or competitive.
485. **Trade-Through Concealment** — Routing, executing, or reporting a trade in a way intended to hide that a better protected quotation was available elsewhere.
486. **Stale-Quote Creation and Exploitation** — Intentionally causing or maintaining stale quotations so the manipulator can trade against them before they update.
487. **Midpoint Reference Manipulation by Venue User** — Manipulating the NBBO or other reference market immediately before midpoint executions to obtain a better dark-pool price.
488. **Duplicate Trade Reporting Manipulation** — Reporting the same execution more than once to inflate apparent trading volume or activity.
489. **Cancelled-Trade Publication Abuse** — Allowing cancelled or broken trades to remain published, or republishing them, to mislead the market.
490. **False Trade Timestamp Reporting** — Intentionally reporting an incorrect execution time to conceal sequencing, lateness, or manipulative coordination.
491. **False Venue Reporting** — Misstating where an execution occurred to hide venue relationships or distort market statistics.
492. **False Capacity Reporting** — Misreporting whether a transaction was agency, principal, or riskless principal to conceal conflicts or compensation.
493. **False Short-Sale Indicator Reporting** — Misreporting the short-sale status of a transaction to evade short-sale rules or surveillance.
494. **False Account-Type Reporting** — Misstating customer, proprietary, market-maker, or other account capacity to avoid controls or hide relationships.
495. **False Order-Origin Reporting** — Misreporting who originated an order or the relevant origin code to conceal the responsible participant.
496. **False Cancel/Correct Report** — Submitting a false cancellation or correction to alter the regulatory record of a trade.
497. **Broken-Trade Concealment** — Failing to correctly reflect a broken or busted trade so published or regulatory records remain misleading.
498. **Block-Size Misreporting** — Intentionally misstating the size of a block transaction to influence market perception or regulatory treatment.
499. **Out-of-Sequence Print Manipulation** — Reporting trades out of sequence with manipulative intent so the tape presents a misleading chronology or price trend.
500. **Off-Exchange Print Price Manipulation** — Using off-exchange trades or reports at selected prices to influence consolidated last sale or reference prices.
501. **Trade-Report Suppression** — Intentionally withholding a reportable execution to hide volume, price impact, or the manipulator's activity.
502. **Phantom Trade Print** — Publishing a trade that did not represent a genuine completed transaction.
503. **Pump-and-Dump Reporting Delay** — Delaying or sequencing trade reports so the manipulator's distribution is less visible during a pump-and-dump.
504. **Average-Price Allocation Concealment** — Using average-price accounts or post-trade allocations to conceal which customer or proprietary account received advantageous executions.
505. **Multi-Account Layering** — Distributing deceptive order layers across several controlled accounts so each account appears less suspicious.
506. **Multi-Broker Spoofing** — Placing deceptive orders through several brokers to hide common control and evade single-broker surveillance.
507. **Multi-Venue Spoofing** — Placing deceptive orders across several venues so the combined false pressure is not visible in a single venue's data.
508. **Beneficial-Owner Cross-Broker Self-Trade** — Using accounts at different brokers to trade against oneself while concealing common beneficial ownership.
509. **Nominee Account Rotation** — Rotating activity among nominee accounts so the same manipulation does not repeatedly appear under one account.
510. **Trader-Identifier Switching** — Changing or rotating trader identifiers to conceal continuity of a manipulative strategy.
511. **Algorithm-Identifier Switching** — Rotating algorithm or strategy identifiers to evade behavior-based surveillance.
512. **Subaccount Cycling** — Moving manipulative activity among subaccounts under a master account to stay below alert thresholds.
513. **Temporal Threshold Evasion** — Spreading related orders or trades over longer intervals so each surveillance window appears benign.
514. **Cross-Security Threshold Evasion** — Distributing a manipulation across correlated securities so activity in any one security remains below alert thresholds.
515. **Common-Funding Coordinated Trading** — Multiple accounts funded from the same source trade in a synchronized manner to disguise common control.
516. **Common-Withdrawal Coordinated Trading** — Multiple apparently independent accounts send trading proceeds to the same destination, indicating hidden coordination.
517. **Synchronized-Access Trading Scheme** — Accounts sharing access patterns or control execute synchronized trades designed to appear independent.
518. **Related-Account Order Relay** — Controlled accounts alternate order placement and cancellation so no single account contains the complete manipulative pattern.
519. **Position-Limit Evasion Through Related Accounts** — Splitting positions among related accounts to avoid aggregation or position limits.
520. **Position-Limit Evasion Through Derivatives** — Using economically equivalent derivatives or linked instruments to conceal a position exceeding applicable limits.
521. **Non-Bona-Fide EFP/EFRP Transaction** — Using an exchange-for-physical or exchange-for-related-position transaction without a genuine corresponding cash or related position.
522. **Prearranged Block-Trade Abuse** — Pre-negotiating a block transaction in a manner that violates competitive or exchange requirements.
523. **Block-Trade Front Running** — Trading before a known block transaction to profit from its anticipated market impact.
524. **Delivery-Squeeze Manipulation** — Controlling deliverable supply or positions near settlement so counterparties must transact at artificial prices.
525. **Forced-Buy-In Manipulation** — Creating or exploiting settlement scarcity to force buy-ins at artificially unfavorable prices.
526. **Free-Float Corner** — Acquiring effective control of most freely tradable shares to manipulate price or pressure other market participants.
527. **Record-Date Squeeze** — Controlling available shares around a dividend, voting, or corporate-action record date to create artificial scarcity.
528. **Ex-Date Price Manipulation** — Trading around an ex-dividend or other ex-date to distort the normal price adjustment for economic benefit.
529. **Options Exercise-Price Manipulation** — Trading the underlying near exercise or assignment cutoffs to affect whether options are exercised or assigned.
530. **Pin-Risk Manipulation** — Keeping an underlying security near an option strike into expiration to influence exercise outcomes and related positions.
531. **Algorithm Gaming** — Designing orders specifically to exploit predictable behavior of another participant's algorithm in a manipulative or deceptive manner.
532. **Algorithmic Momentum Ignition** — Using automated aggressive orders or trades to trigger other algorithms into extending an artificial price move.
533. **Microburst Momentum Ignition** — Creating a very short burst of aggressive activity intended to trigger momentum strategies before quickly reversing.
534. **Order-Anticipation Manipulation** — Detecting a large hidden or algorithmic order and using manipulative trading to move price ahead of its expected executions.
535. **Self-Trade-Prevention Gaming** — Using self-trade prevention settings or related accounts to create misleading order activity while avoiding actual self-execution.
536. **Matching-Engine Stress Manipulation** — Generating excessive or strategically structured messages with the intent to disrupt or degrade fair market processing.
537. **Cancel-Reenter Queue Monopolization** — Repeatedly cancelling and re-entering orders to exploit matching rules and unfairly dominate queue position.
538. **Order-Type Exploitation Manipulation** — Using specialized order types in a deceptive strategy intended to create false signals or obtain an unfair manipulative advantage.
539. **Hidden-Liquidity Algorithm Probing** — Using automated test orders to infer hidden liquidity and then deploying a manipulative strategy against the discovered interest.
540. **Cross-Venue Latency Manipulation** — Coordinating orders across venues with intentional false signals so latency differences create profitable executions elsewhere.


## Suggested Surveillance Families

For implementation, these scenarios can be organized into reusable detector families rather than building one completely separate algorithm for every row:

- Spoofing, layering & deceptive liquidity
- Order-book pressure & quotation manipulation
- Wash, self, matched, prearranged & circular trading
- Price, volume & tape manipulation
- Opening, closing & auction manipulation
- Benchmark, VWAP, TWAP, NAV & settlement manipulation
- Momentum ignition, ramping, pumping & dumping
- Related-account, nominee & coordinated-group behavior
- Cross-broker, cross-venue & cross-market manipulation
- Cross-product, derivatives, ETF/ETP & index manipulation
- Insider dealing & misuse of material non-public information
- Front running, trading ahead & misuse of customer-order information
- Short-selling, locate, borrow & fail-to-deliver abuse
- Securities-lending & settlement manipulation
- Market-maker, liquidity-provider & quote-coordination abuse
- Dark-pool, ATS, internalization & venue-conflict abuse
- Trade-reporting, transaction-publication & identifier manipulation
- Threshold, beneficial-owner & surveillance-evasion behavior
- Position-limit, corner, squeeze & delivery manipulation
- Algorithmic/HFT manipulation, probing & message abuse
- Microcap, low-float, nominee, promotion-linked & hacked-account manipulation
- Account takeover / identity fraud used to execute manipulative trades
- IPO, new-issue, distribution & stabilization trading abuse
- Tender, corporate-action & record-date trading manipulation
- Client/proprietary cross-trade and execution-value-transfer abuse

## Recommended Interpretation for a SMARTS-Like System

Do **not** interpret the number of rows in this file as the number of separate rules you must implement.

A practical surveillance platform should build a smaller set of reusable behavioral detectors, for example:

- cancellation ratio
- order lifetime
- displayed-size anomaly
- multi-level depth pressure
- opposite-side execution
- self/related beneficial owner
- time/price/quantity matching
- circular transaction graph
- price impact
- volume participation
- auction indicative-price impact
- benchmark-window participation
- cross-product economic benefit
- pre-event abnormal trading
- short/borrow/settlement status
- related-account graph
- trade-report timing/accuracy
- cross-venue synchronization
- position concentration
- liquidity concentration
- order-message burst rate
- rapid position reversal

Dynamic rules can then combine those facts into many of the scenarios above.

## Public Research Basis

This revision was expanded and checked against public material from:

- **Nasdaq Market Surveillance (SMARTS)** — public product information describing 70+ preconfigured alerts, full-order-book reconstruction, spoofing scenarios, market manipulation, insider trading, cross-market/cross-asset surveillance, custom alerts, and AI/ML anomaly prioritization.
- **Nasdaq Trade Surveillance (SMARTS)** — public product information describing 300+ behaviors across 200+ markets and coverage including cross-product manipulation, RFQ frontrunning, benchmark fixes, trading-with-knowledge, unusual pricing, and wash-trading surveillance.
- **FINRA 2026 Annual Regulatory Oversight Report — Manipulative Trading** — public examples including layering, spoofing, wash trades, prearranged trades, matched trading, marking the close, odd-lot manipulation, cross-product activity, small-cap pump-and-dump behavior, nominee accounts, and multi-platform monitoring.
- **EU Market Abuse Regulation / Delegated Regulation (EU) 2016/522** — public indicators and examples of manipulative behavior, including false or misleading signals, price securing, matched transactions, and related practices.
- **CFTC public enforcement and trade-practice material** — wash sales, fictitious trades, spoofing/disruptive trading, noncompetitive trading, manipulation, and related trade-practice violations.
- The user's original **380-case Stock-Market Fraud, Manipulation & Surveillance Case Catalog**, which was filtered to retain trading-surveillance-relevant material and remove primarily non-trading fraud domains.

## Version Note

Prepared as an expanded trading-surveillance research catalog on **2026-08-15**. Because SMARTS is proprietary and regulatory typologies evolve, this should be maintained as a living catalog rather than treated as a fixed legal taxonomy.
