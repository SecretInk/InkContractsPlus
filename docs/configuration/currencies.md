---
title: Currencies
parent: Configuration
nav_order: 2
---

# Currencies
{: .no_toc }

Every contract pays in **one currency**, chosen when it's created. Escrow, fees, tax, collateral, refunds and payouts all use that currency. Players only see the currencies they're allowed to use (`inkcontracts.currency.<id>`).

1. TOC
{:toc}

## Top of the file

```yaml
default: money    # used for new drafts and anything priced without a currency
fallback: money   # used when `default` can't load, e.g. Vault isn't installed
```

If a contract's currency is removed from the file later, its escrow is paid out in `fallback`.

## Types

| `type` | Backed by | Notes |
|---|---|---|
| `vault` | Any Vault economy (EssentialsX, CMI, XConomy…) | Picked up automatically even if the economy plugin loads late |
| `internal` | The plugin's own ledger | Works with no other plugins. Can be registered **as** the server's Vault economy |
| `item` | Physical items | Several denominations with change. Whole amounts only |
| `experience` | XP levels or points | The player must be online to pay or be paid |
| `playerpoints` | The PlayerPoints plugin | |
| `command` | Any other plugin | Runs console commands, with a PlaceholderAPI balance |

Other plugins can add their own types through the [API](../developer-api).

## Options for every currency

| Key | |
|---|---|
| `enabled` | `false` switches it off |
| `type` | One of the types above |
| `display-name` | MiniMessage |
| `format` | How amounts are written: `{symbol}`, `{amount}` and `{name}`, e.g. `"{symbol}{amount}"` or `"{amount} {name}"` |
| `symbol`, `name-singular`, `name-plural` | |
| `decimals` | 0 for whole numbers |
| `compact` | `true` writes 4500 as `4.5k` |
| `icon` | Item shown in menus (any [item option](../menus/items-and-buttons#items)) |
| `limits` | Overrides of the `config.yml` money rules, see below |

### Limits

```yaml
limits:
  min-reward: 5
  max-reward: 10000
  fee-percent: 0
  fee-flat: 0
  completion-tax-percent: 0
```

A missing value or `-1` uses `config.yml`. When a [type](types) sets the same limit, the type's value wins.

## vault

```yaml
money:
  type: vault
  provider: ""             # name of the economy plugin, if several register; empty = highest priority
  use-vault-format: true   # use the economy plugin's own formatting instead of `format`
```

## internal

```yaml
credits:
  type: internal
  starting-balance: 100
  max-balance: -1          # -1 = no limit
  transferable: true       # allow /contract pay
  min-transfer: 1
  vault-provider: false    # register as THE server's Vault economy, so shops etc. use it
```

* **Every change is logged:** see `/contract admin eco log <player>`.
* **Staff changes:** `/contract admin eco give|take|set`, which also works on offline players.

{: .warning }
Only enable `vault-provider` when no other economy plugin is installed.

## item

```yaml
emeralds:
  type: item
  mint-tag: false          # true = only coins made by the plugin count (use with custom coin items)
  denominations:
    - { value: 1, material: EMERALD }
    - { value: 9, material: EMERALD_BLOCK }
```

* **Paying:** the largest coins are used first, and change is given back.
* **Denominations:** each one accepts every item option (`item-model`, `custom-model-data`, `name`, `lore`, `glow`…), so resource-pack coins work.
* **Offline payouts:** paid into the player's stash.

## experience

```yaml
xp:
  type: experience
  mode: levels             # levels or points
```

## playerpoints

Needs the PlayerPoints plugin. No extra options.

## command

Use any economy that has console commands:

```yaml
gems:
  type: command
  balance-placeholder: "%gems_balance%"     # PlaceholderAPI, strongly recommended
  withdraw-commands: ["gems take {player} {amount_int}"]
  deposit-commands: ["gems give {player} {amount_int}"]
  requires-online: false   # true if the commands only work on online players
```

Tokens: `{player}`, `{uuid}`, `{amount}`, `{amount_int}`, `{currency}`.

## Pending money

When a payout can't be made, e.g. Vault isn't loaded or the player must be online, the amount is kept as **pending** for that currency. It's paid automatically when the currency is back or the player joins. It's shown in the wallet and as `%inkcontracts_pending_<currency>%`.

## No economy plugin?

```yaml
default: credits
fallback: credits
currencies:
  money:
    enabled: false
```
