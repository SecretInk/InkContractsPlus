---
title: Home
layout: home
nav_order: 1
description: "InkContracts+ is a player-run job board for Paper servers: escrowed rewards, verified work, reputation and fully customisable menus."
permalink: /
---

# InkContracts+
{: .fs-9 }

A player-run job board for your Paper server. Players post paid contracts, other players do the work, and the plugin holds the money, checks the work and pays out.
{: .fs-6 .fw-300 }

[Get started](getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Commands](commands){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What it does

```
CONTRACT #1042  ·  Gathering
Title:    Stone Supply
Issuer:   Northstar Construction   Trusted
Reward:   $4,500   (in escrow)
Deadline: 1d 23h
Status:   OPEN
• Gather 0/2,000 Cobblestone (verified 0)
✔ Verified by the plugin
```

* **[Nine contract types](contracts/types):** Delivery, Gathering, Hunting, Farming, Crafting, Fishing, Construction, Transport and Assistance. All but Assistance are checked by the plugin.
* **[Escrow and a full lifecycle](contracts/lifecycle):** deadlines, partial payouts, disputes, auto-approval and offline payouts.
* **[Reputation](contracts/reputation):** star ratings and tiers from Newcomer to Elite. Posters can require a minimum reputation.
* **[Any economy](configuration/currencies):** Vault, a built-in ledger, physical items, XP, PlayerPoints, or any plugin through commands. Several currencies can run side by side.
* **[Menus you can change completely](menus/):** layouts, click types, conditions, animations and your own menus, all in `guis.yml`.
* **[Storage](configuration/storage):** JSON, SQLite or MySQL, with one-command migration.
* **[PlaceholderAPI](placeholders)** and a **[developer API](developer-api)** with events.

## Requirements

| | |
|---|---|
| Server | Paper 26.3 (or a Paper fork such as Purpur) |
| Java | 25 |
| Optional | Vault + an economy plugin, PlaceholderAPI, PlayerPoints |

## Where to go next

| I want to… | Read |
|---|---|
| Install the plugin and try it | [Getting started](getting-started) |
| Know what players can do | [Contracts](contracts/) and [Commands](commands) |
| Use my server's money | [Currencies](configuration/currencies) |
| Change fees, limits and deadlines | [config.yml](configuration/config) |
| Restyle a menu or make my own | [Menus](menus/) |
| Hook in from my own plugin | [Developer API](developer-api) |
| Fix a problem | [FAQ](faq) |
