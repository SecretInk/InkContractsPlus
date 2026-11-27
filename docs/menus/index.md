---
title: Menus
nav_order: 7
has_children: true
permalink: /menus/
---

# Menus
{: .no_toc }

Every menu is defined in `guis.yml`. You can restyle any of them, move or remove buttons, change what each click does, show things only when they apply, and add menus of your own, without writing code.

1. TOC
{:toc}

## The bundled menus

| Id | Opens with | |
|---|---|---|
| `main` | `/contract` | Hub: board, post, my contracts, wallet, stash, leaderboard, history, profile |
| `board` | `/contract board [type]` | Open contracts with type and currency filters, search, 6 sorts, "hide mine" and reset. Filters are remembered per player |
| `contracts` | `/contract mine` · `history [player]` | Your contracts (toggle finished, sortable), or a player's history |
| `contract` | `/contract view <id>` | One contract: card, timeline, issuer and contractor heads, and every action you can take. Live countdown |
| `stash` | `/contract claim` | Items waiting to be collected |
| `wallet` | `/contract wallet` | Balances in every currency. Right-click a currency to pay someone |
| `templates` | `/contract templates` | Ready-made contracts |
| `leaderboard` | `/contract top` | Top players by reputation |
| `leaderboard-completed` | from the leaderboard | …by contracts completed |
| `leaderboard-posted` | from the leaderboard | …by contracts paid out |
| `package`, `reward-items`, `reward-items-view` | while creating or viewing a contract | Chests players put items into. Only `title` and `rows` can be changed |

`settings.main-menu` decides what `/contract` opens. Set it to `board` for a board-first server.

## How a menu is built

```yaml
menus:
  board:
    title: "<dark_gray>Contract Board · <#8f6a00><count> open[if:pages > 1] (<page>/<pages>)[/if]"
    update-interval: 40               # redraw every 2 seconds (countdowns, balances, the title)
    layout:                           # 1–6 rows of 9 characters
      - "CCCCCCCCC"
      - "CCCCCCCCC"
      - "<#FYSQO#>"
    content: "C"                      # the list (contracts here) fills these slots
    content-click: { left: view, shift-left: accept, right: issuer }
    fill: { preset: border }          # every slot left empty
    items:
      "<": { preset: previous }
      ">": { preset: next }
      "Q":
        click: { left: search, right: clear-search }
        material: SPYGLASS
        name: "[if:searching]Search » \"<search>\"[else]Search"
```

The rest of this section covers each part:

| Page | |
|---|---|
| [Items and buttons](items-and-buttons) | What a slot looks like, and what clicking it does |
| [Conditions and placeholders](conditions) | `[if:…]`, requirements, and the values you can show |
| [Presets and inheritance](reuse) | Define a button once; build menus from other menus |
| [Custom menus](custom-menus) | Your own menus, filled from contracts, templates, currencies or leaderboards |
| [Recipes](recipes) | Copy-paste examples |

## Menu options

| Key | |
|---|---|
| `title` | MiniMessage, with [conditions and placeholders](conditions). It updates while the menu is open |
| `layout` | Rows of 9 characters. Or `rows: 3` for blank rows, with items placed by `slots:` |
| `content` | The layout character the menu's list fills |
| `content-click` | What clicking a list entry does, per click type |
| `items` | Buttons and decoration, by layout character or by slot. See [Items and buttons](items-and-buttons) |
| `fill` | Item for every slot nothing else uses |
| `entry` / `empty` | Look of each list entry, and of the slot shown when the list is empty (custom menus) |
| `update-interval` | Redraw every N ticks while open (20 ticks = 1 second) |
| `permission` | Needed to open the menu |
| `open-requirement` + `deny-message` | A [condition](conditions) that must be true to open it |
| `open-sound` / `close-sound` | Sound names from `config.yml`, or raw keys |
| `open-commands` / `close-commands` | [Steps](items-and-buttons#steps) to run. Close commands only run when the player closes the menu, not when they switch to another one |
| `extends` | Start from another menu or base. See [Presets and inheritance](reuse) |
| `source` / `source-options` | What fills the list. See [Custom menus](custom-menus) |

## Settings

```yaml
settings:
  main-menu: main              # what /contract opens
  lore-wrap: 42                # wrap lore lines longer than this (0 = off); items can override with `wrap:`
  wrap-indent: "  "            # in front of wrapped continuation lines
  remember-board-filters: true # keep board filters, sort and search until the player logs out
```

## Turning menus off and resetting them

* **Turn a bundled menu off:** set it to `false`, e.g. `leaderboard-posted: false`. Don't delete it, because a deleted bundled menu is added back on the next load.
* **Reset a menu to the default:** delete its section and run `/contract admin reload`. The bundled version is added back.
* **Reset everything:** rename `guis.yml` and reload.

## Checking your file

When `guis.yml` loads, and whenever you run `/contract admin menus`, the plugin checks for:
* unknown actions, menus, sources, step types and materials
* layout characters without an item
* slots outside the menu
* items that can't be built

Every problem is printed with the menu and button it's in. The command also lists every menu, action and source.
