---
title: Getting started
nav_order: 2
---

# Getting started
{: .no_toc }

1. TOC
{:toc}

## Install

1. Download the jar from Modrinth.
2. Put it in your server's `plugins/` folder.
3. Start the server. The plugin creates `plugins/InkContractsPlus/` with these files:

| File | What's in it |
|---|---|
| `config.yml` | Fees, limits, deadlines, verification, reputation, notifications, sounds, storage ([reference](configuration/config)) |
| `currencies.yml` | What contracts can pay in ([reference](configuration/currencies)) |
| `types.yml` | Settings for each contract type ([reference](configuration/types)) |
| `templates.yml` | Ready-made contracts ([reference](configuration/templates)) |
| `guis.yml` | Every menu ([reference](menus/)) |
| `messages.yml` | Every chat line ([reference](configuration/messages)) |
| `data/` | Contracts and player profiles (JSON storage) |

## Pick your money

Open `currencies.yml`. The defaults are:

* **With Vault and an economy plugin** (EssentialsX, CMI…): contracts pay in your normal server money (`money`). Nothing to change.
* **Without Vault:** the console shows `Currency 'money' (vault) could not be loaded` and falls back to the built-in **credits**. To make that permanent and silence the warning, set `default: credits` and `fallback: credits`, and set `money` to `enabled: false`.

Emeralds and XP levels are switched on too. Set `enabled: false` on any currency you don't want. See [Currencies](configuration/currencies).

## Try it in game

| Step | Command |
|---|---|
| Open the main menu | `/contract` |
| Post a contract with the wizard | `/contract create` |
| Or start from a template | `/contract templates` |
| See open contracts | `/contract board` |
| See your own | `/contract mine` |

Contracts you post appear on the board for everyone. To test the whole flow alone, join with a second account, accept the contract with it and do the work.

{: .tip }
On a test server with `online-mode=false` you can join with an offline account in a second game window to play both sides.

## Reloading

`/contract admin reload` re-reads every file except `storage.backend`; changing the storage backend needs a restart. Afterwards, run `/contract admin menus` to check `guis.yml` for mistakes.

## Updating the plugin

Replace the jar and restart. New options are added to your files automatically, with their comments. Anything you changed is kept.

Your own currencies, templates, presets, bases and menus are never re-filled from the defaults. A bundled menu you deleted is added back, so to switch one off set it to `false` instead ([details](menus/#turning-menus-off-and-resetting-them)).

## Network use

With the default JSON storage, the plugin downloads nothing. With SQLite or MySQL storage, Paper downloads the HikariCP connection-pool library from Maven Central once, on startup. The plugin never sends any data off your server.
