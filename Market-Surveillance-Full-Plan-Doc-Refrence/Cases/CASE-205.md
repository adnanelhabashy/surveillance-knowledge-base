---
id: CASE-205
type: surveillance-case
case_number: 205
title: "Digital-Signature Trading-Authorization Forgery"
status: implementation-seeded
implementation_archetype: account_security
smarts_public_mapping: not-mapped-from-public-material
tags:
  - surveillance/case
---

# 205. Digital-Signature Trading-Authorization Forgery

## Description

Customer or supervisor signatures are forged on trading authorizations, transaction approvals, wire instructions, or other brokerage documents.

## Surveillance families

- [[Families/FAMILY-08|Related-account, nominee & coordinated-group behavior]]

## Reusable detector starting points

- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]]
- [[Detectors/DETECTOR-16|Related-Account Graph]]
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]]

## Related cases

- [[Cases/CASE-249|Improper Agency-Cross Transaction]]
- [[Cases/CASE-101|Misappropriation of Customer Assets]]
- [[Cases/CASE-521|Non-Bona-Fide EFP/EFRP Transaction]]
- [[Cases/CASE-169|Fake Share-Provenance / Legal-Opinion Scheme]]
- [[Cases/CASE-416|Abusive Pre-Hedging]]

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
