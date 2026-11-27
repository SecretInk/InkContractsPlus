---
title: Recipes
parent: Menus
nav_order: 5
---

# Recipes
{: .no_toc }

Copy-paste examples. Put them under `menus:` in `guis.yml` (presets under `presets:`), then run `/contract admin reload` and `/contract admin menus`.

1. TOC
{:toc}

## Make the board what /contract opens

```yaml
settings:
  main-menu: board
```

## A board for one type, best-paid first

```yaml
menus:
  hunting-board:
    extends: paged-list
    title: "<red>Hunting jobs <dark_gray>· <page>/<pages>"
    source: contracts
    source-options: { type: HUNTING, sort: reward-high, hide-own: true }
    content-click: { left: view, shift-left: accept }
    empty: { material: BONE, name: "<gray>No hunting jobs right now" }
```

To add a button for it to the main menu, give a free layout character in `main` an item:

```yaml
"H":
  action: open:hunting-board
  material: IRON_SWORD
  name: "<red>Hunting jobs <dark_gray>(<board_count:HUNTING>)"
```

## A richest-players board for a currency

```yaml
menus:
  top-earners:
    extends: leaderboard
    title: "<gold>Top earners"
    source-options: { stat: "earned:money", limit: 28, min-value: 1 }
```

## A staff disputes menu

```yaml
menus:
  disputes:
    extends: paged-list
    title: "<red>Open disputes"
    permission: inkcontracts.admin
    source: contracts
    source-options: { scope: disputed, sort: oldest }
```

Open it with `/contract menu disputes`. Resolving is done on the contract page.

## A VIP-only button

```yaml
"V":
  material: NETHER_STAR
  name: "<light_purple>VIP contracts"
  view-requirement: "perm:myserver.vip"
  action: open:vip-board
```

## Show a different item when the player can't afford something

```yaml
"P":
  - view-requirement: "balance:credits >= 500"
    material: EMERALD
    name: "<green>Buy a boost <gray>(500 credits)"
    confirm: { title: "Buy a boost?", text: "This costs 500 credits.", yes: "Buy" }
    click: { left: "[console] boost give {player}" }
  - material: GRAY_DYE
    name: "<gray>Buy a boost <red>(you need 500 credits)"
```

## Animated border

```yaml
presets:
  border:
    material: BLACK_STAINED_GLASS_PANE
    hide-tooltip: true
    frames:
      - { material: GRAY_STAINED_GLASS_PANE }
      - { material: BLACK_STAINED_GLASS_PANE }
    frame-ticks: 20
```

Every menu that uses the `border` preset now pulses.

## Resource-pack buttons

```yaml
presets:
  next:
    material: PAPER
    item-model: "myserver:gui/arrow_right"
    tooltip-style: "myserver:gold"
```

## Run something when a menu opens

```yaml
menus:
  main:
    open-commands: ["[sound] minecraft:block.chest.open 0.5 1.4"]
    close-commands: ["[actionbar] <gray>See you on the job board!"]
```
