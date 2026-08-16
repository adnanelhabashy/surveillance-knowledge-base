---
id: DETECTOR-20
type: reusable-detector
title: "Liquidity Concentration"
status: concept
tags:
  - surveillance/detector
---

# Liquidity Concentration

Reusable behavioral fact/detector proposed by the source catalog for combination inside dynamic surveillance rules.

## Candidate cases

- [[Cases/CASE-018|Odd-Lot Manipulation]]
- [[Cases/CASE-020|Pegging Manipulation]]
- [[Cases/CASE-021|Price Capping]]
- [[Cases/CASE-024|Quote Stuffing]]
- [[Cases/CASE-025|Order Stuffing]]
- [[Cases/CASE-026|Excessive Order Cancellation Manipulation]]
- [[Cases/CASE-027|Flickering Orders]]
- [[Cases/CASE-030|Order-Book Imbalance Manipulation]]
- [[Cases/CASE-031|Dominant Bid Manipulation]]
- [[Cases/CASE-032|Dominant Offer Manipulation]]
- [[Cases/CASE-033|Bid-Ask Spread Manipulation]]
- [[Cases/CASE-050|Short and Distort]]
- [[Cases/CASE-051|Trash and Cash]]
- [[Cases/CASE-075|False Social-Media Information]]
- [[Cases/CASE-088|Cornering the Market]]
- [[Cases/CASE-089|Market Squeeze]]
- [[Cases/CASE-128|Rumor + Trading Manipulation]]
- [[Cases/CASE-134|Smoking]]
- [[Cases/CASE-135|Phishing / Order-Book Phishing]]
- [[Cases/CASE-136|Flying]]
- [[Cases/CASE-138|Trading-Safeguard Bypass Manipulation]]
- [[Cases/CASE-144|Advancing the Bid]]
- [[Cases/CASE-146|Restricted-Stock Loan Default Scheme]]
- [[Cases/CASE-153|Small-Purchase Price Support for Large Inventory]]
- [[Cases/CASE-160|Alternative Merger / Exchange-Offer Price Manipulation]]
- [[Cases/CASE-161|Pre-Offering Short-Sale / Rule 105 Abuse]]
- [[Cases/CASE-167|Quotation-Reactivation Manipulation]]
- [[Cases/CASE-183|Fake Tender-Offer / Regulatory-Filing Manipulation]]
- [[Cases/CASE-189|Improper Price Stabilization]]
- [[Cases/CASE-193|Engineered Short Squeeze]]
- [[Cases/CASE-200|Distribution Restricted-Period Manipulation]]
- [[Cases/CASE-208|Fictitious Quotation]]
- [[Cases/CASE-210|Closing-Bid Marking]]
- [[Cases/CASE-211|False Appearance of Customer Interest]]
- [[Cases/CASE-212|Quote Coordination Between Market Participants]]
- [[Cases/CASE-214|Pressure-to-Alter-Quote Scheme]]
- [[Cases/CASE-215|Market-Maker Retaliation / Intimidation]]
- [[Cases/CASE-218|Non-Firm Stated-Price Offer]]
- [[Cases/CASE-235|Stock-Loan Sham Finder-Fee Scheme]]
- [[Cases/CASE-236|Stock-Loan Kickback Scheme]]
- [[Cases/CASE-238|Trade Shredding]]
- [[Cases/CASE-245|Payment-for-Order-Flow Biased Routing]]
- [[Cases/CASE-246|Maker-Taker Rebate Biased Routing]]
- [[Cases/CASE-251|Fully Paid Securities Lending Without Proper Consent or Disclosure]]
- [[Cases/CASE-252|Unsuitable Fully Paid Securities Lending]]
- [[Cases/CASE-255|Stop-Loss Trigger Manipulation]]
- [[Cases/CASE-256|Tender-Offer Outside-Purchase / Unequal-Consideration Abuse]]
- [[Cases/CASE-257|Short Tendering]]
- [[Cases/CASE-269|Retail Execution-Quality Misrepresentation]]
- [[Cases/CASE-287|Securities-Loan Quantity Misreporting]]
- [[Cases/CASE-288|Securities-Loan Rate / Fee Misreporting]]
- [[Cases/CASE-299|Moving-Wall Manipulation]]
- [[Cases/CASE-300|Fake Support Wall]]
- [[Cases/CASE-301|Fake Resistance Wall]]
- [[Cases/CASE-302|Synthetic Depth Creation]]
- [[Cases/CASE-303|Depth Withdrawal Manipulation]]
- [[Cases/CASE-305|Quote-Fade Manipulation]]
- [[Cases/CASE-306|Bait-and-Switch Quoting]]
- [[Cases/CASE-313|Odd-Lot Quote Shaping]]
- [[Cases/CASE-317|Cross-Symbol Quote Stuffing]]
- [[Cases/CASE-318|Feed-Delay Quote Stuffing]]
- [[Cases/CASE-319|Locked-Market Inducement]]
- [[Cases/CASE-320|Crossed-Market Inducement]]
- [[Cases/CASE-321|Spread-Widening Manipulation]]
- [[Cases/CASE-322|Spread-Narrowing Manipulation]]
- [[Cases/CASE-323|One-Sided Book Pressure Manipulation]]
- [[Cases/CASE-324|Book-Pressure Flip Manipulation]]
- [[Cases/CASE-325|Order-Ladder Manipulation]]
- [[Cases/CASE-326|Reserve-Order Replenishment Manipulation]]
- [[Cases/CASE-328|Midpoint-Peg Manipulation]]
- [[Cases/CASE-329|Pegged-Order Reference Manipulation]]
- [[Cases/CASE-338|Quote-and-Trade Combination Manipulation]]
- [[Cases/CASE-372|Late Auction Cancellation Abuse]]
- [[Cases/CASE-402|Cross-Border Price-Lead Manipulation]]
- [[Cases/CASE-404|Gamma-Squeeze Manipulation Scheme]]
- [[Cases/CASE-418|Front Running Offerings]]
- [[Cases/CASE-421|Trading with Knowledge of Auction Imbalance]]
- [[Cases/CASE-422|Trading with Knowledge of Order Imbalance]]
- [[Cases/CASE-423|Misuse of Customer IOI Information]]
- [[Cases/CASE-433|Long-Sale Mismarking]]
- [[Cases/CASE-442|Stock-Borrow Squeeze Manipulation]]
- [[Cases/CASE-443|Borrow-Fee Manipulation]]
- [[Cases/CASE-446|Recall-Timing Manipulation]]
- [[Cases/CASE-448|Borrow-Inventory Corner]]
- [[Cases/CASE-449|Short-Squeeze Ignition]]
- [[Cases/CASE-455|Low-Float Squeeze Pump]]
- [[Cases/CASE-471|Liquidity-Program Gaming]]
- [[Cases/CASE-472|Coordinated Quote Widening]]
- [[Cases/CASE-473|Coordinated Quote Narrowing]]
- [[Cases/CASE-474|Market-Maker Quote Synchronization]]
- [[Cases/CASE-475|Fake Two-Sided Market]]
- [[Cases/CASE-476|Skewed Two-Sided Quote Manipulation]]
- [[Cases/CASE-485|Trade-Through Concealment]]
- [[Cases/CASE-486|Stale-Quote Creation and Exploitation]]
- [[Cases/CASE-494|False Account-Type Reporting]]
- [[Cases/CASE-495|False Order-Origin Reporting]]
- [[Cases/CASE-519|Position-Limit Evasion Through Related Accounts]]
- [[Cases/CASE-524|Delivery-Squeeze Manipulation]]
- [[Cases/CASE-525|Forced-Buy-In Manipulation]]
- [[Cases/CASE-526|Free-Float Corner]]
- [[Cases/CASE-527|Record-Date Squeeze]]
- [[Cases/CASE-538|Order-Type Exploitation Manipulation]]

## Implementation workspace

- **Inputs:** To define
- **State/window:** To define
- **Output fact:** To define
- **Orleans owner:** To define
- **Calibration:** To define

## Navigation

- [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
- [[00 - Project Home|Project Home]]
