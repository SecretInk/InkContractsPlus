---
title: Commands
nav_order: 4
---

# Commands
{: .no_toc }

The root command is `/contract`, with the aliases `/contracts` and `/jobs`. Everything tab-completes, including item, mob and block ids, contract ids and currencies.

1. TOC
{:toc}

## Menus

| Command | |
|---|---|
| `/contract` | Open the main menu (`settings.main-menu` in guis.yml) |
| `/contract board [type]` | The contract board, optionally filtered to one type |
| `/contract mine` | Your contracts |
| `/contract history [player]` | Finished contracts, yours or another player's |
| `/contract wallet` | Your balances in every currency |
| `/contract claim` | Your stash of items waiting to be collected |
| `/contract templates` | Ready-made contracts |
| `/contract top` | The leaderboard |
| `/contract menu [id]` | Open any menu by id, or list the menus you can open |

## Posting

| Command | |
|---|---|
| `/contract create` | The creation wizard |
| `/contract template <id>` | Start a draft from a template |
| `/contract draft …` | Edit your draft. See [Creating contracts](contracts/creating#drafts) |

## Working

| Command | |
|---|---|
| `/contract view <id>` · `info <id>` | Open a contract (`info` prints it in chat, also from the console) |
| `/contract accept <id>` | Take a contract |
| `/contract deliver [id]` | Hand in items, or deliver a transport package at the destination |
| `/contract submit <id>` | Submit Construction or Assistance work |
| `/contract abandon <id>` | Give a contract back |

## As the issuer

| Command | |
|---|---|
| `/contract approve <id>` · `reject <id>` | Review submitted work |
| `/contract cancel <id>` | Withdraw a contract |

## Both sides

| Command | |
|---|---|
| `/contract dispute <id> [reason]` | Freeze the contract for staff |
| `/contract rate <id> <1-5>` | Rate the other side |
| `/contract rep [player]` | Reputation and stats |

## Money

| Command | |
|---|---|
| `/contract balance [player]` | Balances in every currency (`[player]` needs `inkcontracts.wallet.others`) |
| `/contract currencies` | List the currencies |
| `/contract pay <player> <amount> [currency]` | Send money. Only currencies with `transferable: true` |

## Staff

All of these need `inkcontracts.admin`.

| Command | |
|---|---|
| `/contract admin reload` | Re-read every file (not the storage backend) |
| `/contract admin save` | Save all data now |
| `/contract admin disputes` | List open disputes |
| `/contract admin resolve <id> contractor\|issuer\|split [percent]` | Settle a dispute ([details](contracts/lifecycle#disputes)) |
| `/contract admin cancel <id>` | Force-cancel a contract |
| `/contract admin eco give\|take\|set <player> <currency> <amount>` | Change internal-currency balances, including offline players |
| `/contract admin eco log <player>` | A player's ledger history |
| `/contract admin eco top <currency>` | Richest players in a currency |
| `/contract admin migrate <json\|sqlite\|mysql>` | Copy all data to another storage backend ([details](configuration/storage)) |
| `/contract admin menus` | List every menu, action and source, and check guis.yml for mistakes |
