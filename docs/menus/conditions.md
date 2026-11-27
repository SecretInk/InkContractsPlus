---
title: Conditions and placeholders
parent: Menus
nav_order: 2
---

# Conditions and placeholders
{: .no_toc }

1. TOC
{:toc}

## Inline conditions

These work anywhere in titles, names and lore:

```yaml
name: "[if:searching]Search » \"<search>\"[else]Search"
lore:
  - "[if:urgent]<red>⚠ [/if]<white><time>"           # part of a line
  - "[if:min-rep]<gray>Needs <yellow><min_rep> rep"  # a whole line: removed when false
  - "[if:open][if:!issuer]<green>Click to accept[/if][/if]"   # nested
```

* `[if:expr]` without `[/if]` runs to the end of the line.
* A lore line whose conditional content is all hidden disappears, so there are no gaps.

## Requirements

The same expressions are used by:
* `view-requirement`: the button is hidden unless it's true
* `click-requirement`: the click is refused unless it's true
* `open-requirement`: the menu won't open unless it's true

## Expressions

| Form | Example |
|---|---|
| Flag | `open`, `has-draft` |
| Not | `!issuer` |
| And | `open & !issuer` (or `&&`) |
| Or | `transport \| construction` (or `\|\|`) |
| Brackets | `(transport \| construction) & urgent` |
| Compare numbers | `reputation >= 50`, `balance:credits > 100`, `pages > 1` |
| Compare text | `menu = leaderboard`, `sort != "Highest reward"` (case-insensitive) |
| PlaceholderAPI | `%vault_eco_balance% >= 1000`, `%player_world% = world_nether` |

* **Numbers:** comparisons ignore thousands separators, so `1,250` reads as 1250.
* **Values used as flags:** a value counts as a true flag when it's a positive number, so `stash` means "has items in the stash".
* **Mistakes:** a malformed expression counts as false, and `/contract admin menus` reports it.

## Flags

Everywhere:

| Flag | True when |
|---|---|
| `perm:<node>` | The player has the permission |
| `op` | The player is an operator |
| `has-draft` | The player has an unpublished draft |
| `world:<name>` / `gamemode:<mode>` | The player is in that world or game mode |
| `currency:<id>` | The player may use that currency |
| `type-enabled:<type>` | The contract type is on |
| `can-post` / `can-post:<type>` | The player may post (that type) |
| `menu:<id>` | This menu is `<id>` |
| `has-back` / `first-page` / `last-page` / `empty` | Navigation state |

Each menu has more. They are listed above each menu in `guis.yml`, e.g. the board has `searching`, `filtered`, `hide-own` and `multi-currency`. On contract cards and the contract page:

`open` `active` `submitted` `disputed` `finished` `has-contractor` `has-label` `transport` `construction` `verified` `urgent` `min-rep` `reward-items` `mine` `issuer` `contractor` `click`

## Values

Values can be compared (`>= 10`) and shown in `amount:`.

| Value | |
|---|---|
| `page`, `pages`, `total`, `menu` | |
| `reputation`, `completed`, `failed`, `abandoned`, `issued`, `rating` | The viewer's stats |
| `balance` / `balance:<currency>` | Balance (default currency, or a specific one) |
| `earned:<c>`, `spent:<c>`, `pending:<c>` | |
| `stash` | Stacks waiting in the stash |
| `active`, `posted`, `waiting` | Contracts in progress, posted and open, and waiting for your review |
| `board` / `board:<type>` | Open contracts on the board |
| `level`, `online` | XP level, players online |

On contract cards: `id`, `reward`, `percent`, `min-rep`, `hours-left`, `minutes-left`, `type`, `status`, `currency`.

## Placeholders

Placeholders are written `<name>`. These work in every menu:

`<player>` `<reputation>` `<tier>` `<stash>` `<page>` `<pages>` `<total>` `<menu>` `<completed>` `<failed>` `<abandoned>` `<issued>` `<issued_completed>` `<rating>` `<ratings>` `<balance>` `<balance:CURRENCY>` `<board_count>` `<board_count:TYPE>` `<active_count>` `<posted_count>` `<waiting_count>` `<earned_in:CURRENCY>` `<currency_name:CURRENCY>`

**Multi-line placeholders** expand to several lines when a lore line is exactly that tag: `<balances>`, `<earned>`, and on contracts `<requirements>`.

**Contract cards** (`contract-card` in guis.yml):

`<id>` `<title>` `<type>` `<type_colored>` `<issuer>` `<issuer_player>` `<tier>` `<reward>` `<reward_total>` `<currency>` `<time>` `<status>` `<contractor>` `<min_rep>` `<progress>` `<percent>` `<reward_items>` `<collateral>` `<destination>` `<region>`

**Each menu** adds its own placeholders, listed above it in `guis.yml`, e.g. `<filter>`, `<sort>` and `<search>` on the board.

**PlaceholderAPI** `%placeholders%` work in titles, names, lore, conditions and commands when PlaceholderAPI is installed.

## The progress bar

`<progress>` on contracts is drawn from `contract-card.progress-bar`:

```yaml
progress-bar:
  width: 20
  symbol: "■"
  complete: "<green>"
  incomplete: "<dark_gray>"
```
