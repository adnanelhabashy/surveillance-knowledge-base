---
id: CASE-194
type: surveillance-case
case_number: 194
title: "Hacked-Account Forced-Buy Pump"
status: implementation-seeded
implementation_archetype: account_security
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 194. Hacked-Account Forced-Buy Pump

## Description

Compromised brokerage accounts are forced to buy thinly traded securities, driving up price and volume so fraudsters can sell at inflated prices.

## Surveillance families

- [[Families/FAMILY-07|Momentum ignition, ramping, pumping & dumping]]
- [[Families/FAMILY-21|Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]]

## Reusable detector starting points

- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-10|Volume Participation]]
- [[Detectors/DETECTOR-22|Rapid Position Reversal]]
- [[Detectors/DETECTOR-19|Position Concentration]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]

## Related cases

- [[Cases/CASE-525|Forced-Buy-In Manipulation]]
- [[Cases/CASE-154|Deposit–Sell–Wire-Out Scheme]]
- [[Cases/CASE-048|Pump and Dump]]
- [[Cases/CASE-207|Direct-To-Investor Stock-Manipulation Scam]]
- [[Cases/CASE-053|Microcap / Penny-Stock Manipulation]]

## SMARTS mapping

- **Public mapping:** Not mapped to an explicitly named Nasdaq SMARTS behavior from the public material used by the source catalog.
- Keep this as a surveillance requirement candidate rather than claiming SMARTS coverage.

## Implementation workspace

- **Rule status:** Initial contextual deterministic model
- **Detection mode:** Rules + account-security/authentication data; AI optional
- **Rule logic (starter):** Flag trading inconsistent with the account’s normal behavior or authorization context, especially new device/identity, sudden illiquid-security concentration, uneconomic cross-trades, rapid liquidation or immediate cash/asset movement.
- **Orleans grains/state:** AccountGrain, InvestorGrain, OrderGrain, PositionGrain, RelationshipGrain, SurveillanceGrain; maintain behavioral baseline, login/device signals, authorization state and destination accounts
- **Required event fields:** accountId, investorId, eventTime, instrumentId, side, price, quantity, orderType, device/session/IP if available, authentication/authorization event, newPayee/destination, position change, transfer/wire events, counterparty account
- **Time window(s):** Real-time order check; 1–60 min session; 1–5 day takeover/liquidation pattern; 30–90 day behavioral baseline
- **Thresholds/calibration:** Start with new/risky session plus trade size > 5× normal, illiquid-security concentration > 20% of account, uneconomic price deviation > 5%, or rapid sell/withdrawal within 1 day. Security team should tune risk-score thresholds.
- **Alert evidence:** Authentication/session evidence; authorization state; abnormality vs account baseline; exact orders/trades; counterparties; asset/cash transfer trail; device/IP changes
- **Implementation note:** Starter engineering model only. Calibrate by instrument liquidity, session phase, participant type and historical percentiles before production use.

## Source

- [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]
