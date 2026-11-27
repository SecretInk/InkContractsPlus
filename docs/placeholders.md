---
title: PlaceholderAPI
nav_order: 8
---

# PlaceholderAPI

With [PlaceholderAPI](https://modrinth.com/plugin/placeholderapi) installed, these work in scoreboards, tab lists, chat and other plugins. They also work inside InkContracts+ menus and conditions.

## Player

| Placeholder | |
|---|---|
| `%inkcontracts_reputation%` | Reputation score |
| `%inkcontracts_tier%` / `%inkcontracts_tier_plain%` | Tier name, with or without colours |
| `%inkcontracts_completed%`, `_failed%`, `_abandoned%` | As contractor |
| `%inkcontracts_issued%`, `_issued_completed%` | As issuer |
| `%inkcontracts_rating%` / `%inkcontracts_issuer_rating%` | Average stars as contractor / as issuer |
| `%inkcontracts_disputes_won%`, `_disputes_lost%` | |
| `%inkcontracts_stash%` | Stacks waiting in the stash |

## Money

| Placeholder | |
|---|---|
| `%inkcontracts_balance_<currency>%` | Formatted balance, e.g. `%inkcontracts_balance_credits%` → `1,250 Credits` |
| `%inkcontracts_balance_raw_<currency>%` | Plain number |
| `%inkcontracts_earned_<currency>%`, `_spent_<currency>%` | Earned from and spent on contracts |
| `%inkcontracts_pending_<currency>%` | Owed, waiting to be paid |

## Contracts

| Placeholder | |
|---|---|
| `%inkcontracts_active%` | Contracts the player is working on |
| `%inkcontracts_posted_open%` | The player's posted, unfinished contracts |
| `%inkcontracts_board_open%` / `%inkcontracts_board_open_<type>%` | Open contracts on the board (of one type) |
| `%inkcontracts_active_<n>_title%` | Title of the player's *n*-th active contract (1 = first) |
| `%inkcontracts_active_<n>_id%`, `_progress%`, `_time%`, `_reward%` | Its id, progress, time left and reward |

A scoreboard example (most scoreboard plugins):

```
&6Contract: &f%inkcontracts_active_1_title%
&7Progress: &f%inkcontracts_active_1_progress%
&7Time left: &f%inkcontracts_active_1_time%
```

## Placeholder-based currencies

A `command` currency reads its balance from a PlaceholderAPI placeholder (`balance-placeholder`). See [Currencies](configuration/currencies#command).
