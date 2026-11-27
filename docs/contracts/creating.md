---
title: Creating contracts
parent: Contracts
nav_order: 2
---

# Creating contracts
{: .no_toc }

1. TOC
{:toc}

## The wizard

`/contract create`, or the **Post a contract** button in the main menu, opens the creation wizard. It uses Minecraft's native dialog screens, so players fill in fields instead of typing commands:

1. **The basics:**
   * the **type** (only enabled types the player may post)
   * the title, and an issuer label if the player has `inkcontracts.label`
   * the **currency** (shown when the player can use more than one)
   * the reward (`4500` or `4.5k`), the deadline and a minimum contractor reputation
2. **Objectives:** items, mobs or blocks with an amount, or the task text for Assistance.
3. **Review**, then **publish**.

When a contract is published, the reward and the listing fee are taken from the poster. See [Fees](#fees-and-limits).

## Drafts

Until it's published, a contract is a **draft**. Players can close the wizard and come back. Everything can also be done by command:

| Command | |
|---|---|
| `/contract draft` / `draft review` | Show the draft |
| `/contract draft publish` · `draft cancel` | Publish, or throw the draft away |
| `/contract draft currency <id>` | Change the currency |
| `/contract draft add item\|mob\|block <id> <amount>` | Add an objective (tab-completes every registry id) |
| `/contract draft remove <n>` | Remove objective number *n* |
| `/contract draft task <text>` | Task text (Assistance) |
| `/contract draft requirements` | Edit objectives |
| `/contract draft rewards` | Open the **bonus item** editor |
| `/contract draft package` · `draft destination` | Transport package and destination |
| `/contract draft pos1` · `draft pos2` | Construction region corners |

## Templates

`/contract templates` opens a menu of ready-made contracts, and `/contract template <id>` starts one directly. A template fills in a draft, and the player can change anything before publishing. Server owners define templates in [templates.yml](../configuration/templates).

## Bonus item rewards

When `listing.reward-items.enabled` is on, the poster can put up to `max-stacks` stacks of items in escrow as a bonus on top of the reward. Players add them with `/contract draft rewards`. Items in `blacklist` are refused. The contractor gets the items on completion; otherwise they go back to the poster.

## Fees and limits

* **Listing fee:** `max(fee-flat, reward × fee-percent / 100)`. It is taken when the contract is published and never refunded.
* **Completion tax:** `completion-tax-percent` is taken from the contractor's payout.
* **Reward range:** `min-reward` / `max-reward`.

These, and the deadline range, objective limits and the number of open and active contracts per player, are set in `config.yml` → `listing` and `contracts`. Each currency and each type can override the money limits. When both do, the **type** wins, then the **currency**, then `config.yml`.

Players with `inkcontracts.bypass.limits` ignore the open/active contract limits.

## Issuer labels

With `inkcontracts.label`, a player can post under a custom name such as a company. The board shows the label, followed by the real player name in brackets.
