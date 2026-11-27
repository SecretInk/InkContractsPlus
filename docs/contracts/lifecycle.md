---
title: Lifecycle and escrow
parent: Contracts
nav_order: 3
---

# Lifecycle and escrow
{: .no_toc }

1. TOC
{:toc}

## Statuses

```
OPEN ──accept──▶ ACTIVE ──(submit)──▶ SUBMITTED ──approve──▶ COMPLETED
  │                │                      │
  ▼                ▼                      ▼
EXPIRED        FAILED / back to OPEN   DISPUTED ──staff──▶ COMPLETED / FAILED
CANCELLED      DISPUTED
```

| Status | Meaning |
|---|---|
| `OPEN` | On the board, waiting for a contractor |
| `ACTIVE` | Someone is working on it |
| `SUBMITTED` | Construction or Assistance work is waiting for the issuer to review it |
| `COMPLETED` | Done and paid |
| `EXPIRED` | Nobody took it before the deadline |
| `CANCELLED` | The issuer withdrew it |
| `FAILED` | The contractor missed the deadline, or staff settled a dispute for the issuer |
| `DISPUTED` | Frozen until staff decide |

## Where the money goes

| Event | What happens |
|---|---|
| **Publish** | The reward and the listing fee are taken from the issuer. The reward is held in **escrow**; the fee is gone |
| **Complete** | The escrow is paid to the contractor, minus the optional completion tax. Bonus items go to the contractor |
| **Expires while open** | Full refund to the issuer, and any transport package goes back to the issuer's stash |
| **Deadline missed while active** | With `partial-payout: true`, the contractor is paid for what they already delivered and the rest is refunded. The contractor loses reputation. Transport collateral goes to the issuer |
| **Contractor abandons** | Delivered goods are paid pro rata and the contract goes back on the board. The contractor waits `abandon-cooldown-minutes` before accepting another one and loses reputation |
| **Issuer cancels (open)** | Full refund |
| **Issuer cancels (active)** | Only if `issuer-cancel-active: true`. The contractor is paid for deliveries plus `issuer-cancel-compensation-percent` of the remaining escrow. The issuer loses reputation |

All of this happens in the contract's own currency: escrow, fees, tax, collateral, refunds and payouts.

## Submitting and reviewing

Construction and Assistance contracts are **submitted** (`/contract submit <id>`). The issuer then approves or rejects them.

* **Approve:** the contract completes and pays out.
* **Reject:** the contract goes back to `ACTIVE`. If the deadline has nearly passed, it is extended so the contractor can fix the work.
* **No response:** after `contracts.review-hours`, the work is approved automatically, so an absent issuer can't hold back payment.

## Disputes

Either side can freeze an active or submitted contract with `/contract dispute <id> [reason]`. Players with `inkcontracts.notify.disputes` are alerted. Staff list disputes with `/contract admin disputes`, then settle each one:

```
/contract admin resolve <id> contractor        complete it: the contractor is paid in full
/contract admin resolve <id> issuer            fail it: deliveries are paid pro rata, the rest is refunded
/contract admin resolve <id> split [percent]   pay the contractor <percent> (default 50), refund the rest
```

With `contractor` or `issuer`, the losing side gets the `dispute-lost` reputation penalty, and with `issuer` the contractor also gets a failure on record. A split counts as completed with no penalty, and bonus items go to the contractor when they get 50% or more.

## Offline players

* **Messages** are queued as mail and shown when the player joins.
* **Items** (deliveries, packages, bonus items, item-currency payouts) go to the player's **stash**. Players collect them with `/contract claim` or the stash menu.
* **Money** that couldn't be paid, e.g. because Vault wasn't loaded or the currency needs the player online, is kept as **pending** for that currency. It is paid automatically when the currency is back or the player joins.

## Clean-up

Finished contracts older than `contracts.prune-after-days` are removed from storage. Players' stats and reputation are kept in their profiles.
