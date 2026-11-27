---
title: Configuration
nav_order: 6
has_children: true
permalink: /configuration/
---

# Configuration

All files are in `plugins/InkContractsPlus/`. `/contract admin reload` applies changes; only the storage backend needs a restart.

| File | Page |
|---|---|
| `config.yml` | [Main config](config): listing rules, contracts, verification, reputation, notifications, sounds, storage |
| `currencies.yml` | [Currencies](currencies) |
| `types.yml` | [Contract types](types) |
| `templates.yml` | [Templates](templates) |
| `messages.yml` | [Messages](messages) |
| `guis.yml` | [Menus](../menus/) |
| storage | [Storage](storage) |

## Updating

When the plugin updates, new options are added to your files with their comments, and the console says `Added new default options to <file>`. Your values are never changed.

Lists you own are never re-filled from the defaults, so entries you deleted stay deleted:
* currencies in `currencies.yml`
* templates in `templates.yml`
* presets, bases and menus in `guis.yml`

Text uses [MiniMessage](https://docs.advntr.dev/minimessage/format.html) everywhere: `<red>`, `<#f5c542>`, `<bold>`, `<gradient:gold:yellow>` and so on.
