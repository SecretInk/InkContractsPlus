---
title: Custom menus
parent: Menus
nav_order: 4
---

# Custom menus
{: .no_toc }

Any new id under `menus:` is a custom menu. Open it with:
* the action `open:<id>` on a button
* `/contract menu <id>`
* `settings.main-menu`, to make it what `/contract` opens

1. TOC
{:toc}

## A menu of buttons

```yaml
menus:
  info:
    title: "<gold>Server jobs"
    layout:
      - "#########"
      - "#  R D  #"
      - "####X####"
    fill: { preset: border }
    items:
      "R":
        material: BOOK
        name: "<yellow>Rules"
        click: { any: "[player] rules" }
      "D":
        material: DIAMOND
        name: "<aqua>Donate"
        click: { any: ["[close]", "[message] <aqua>Visit store.example.com"] }
      "X": { preset: close }
```

## Sources

`source:` fills the `content` slots with a list. Pages, the `<page>`/`<pages>` placeholders and the `empty` flag work automatically.

| Source | `source-options` | Each entry looks like |
|---|---|---|
| `contracts` | see below | `contract-card` |
| `templates` | – | `entry:` |
| `currencies` | – | `entry:` |
| `leaderboard` | `stat`, `limit`, `min-value` | `entry:` |

`empty:` is the item shown when the list is empty.

### contracts

| Option | Values |
|---|---|
| `scope` | `board`: open contracts (the default)<br>`mine`: unfinished ones you posted or work on<br>`mine-all`: everything you posted or worked on<br>`active`: ones you're working on<br>`posted`: unfinished ones you posted<br>`history`: your finished contracts, newest first<br>`all-open`: every unfinished contract<br>`disputed`: disputed contracts (for staff) |
| `type` | A contract type, e.g. `HUNTING` |
| `currency` | A currency id |
| `sort` | `newest` · `oldest` · `reward-high` · `reward-low` · `ending` · `progress` |
| `search` | Text to match |
| `hide-own` | `true` hides the viewer's own contracts |

`content-click` can be `view` (the default), `accept`, `issuer`, `none`, or a command step with `{id}`, e.g. `"[player] contract info {id}"`.

### templates

Templates the viewer can use. Entry placeholders:
* `<template>`, `<title>`, `<type>`, `<type_colored>`, `<reward>`, `<currency>`, `<deadline>`, `<deadline_text>`
* multi-line: `<requirements>`, `<description>`

### currencies

The viewer's wallet. Entry placeholders:
* `<currency>`, `<currency_id>`, `<balance>`, `<earned>`, `<spent>`, `<pending>`, `<type>`
* flags: `pending`, `default`, `transferable`

`content-click: pay` asks for a player and an amount. A command step can use `{currency}`.

### leaderboard

| Option | |
|---|---|
| `stat` | `reputation` · `completed` · `issued` · `issued-completed` · `rating` · `earned:<currency>` |
| `limit` | How many players |
| `min-value` | Leave out players below this |

Entry placeholders:
* `<rank>`, `<name>`, `<value>`, `<stat>`, `<entry_reputation>`, `<entry_tier>`, `<entry_completed>`, `<entry_issued>`, `<entry_rating>`
* flags: `self`, `top-1`, `top-3`

`content-click` defaults to `history` (open that player's history); `none` or `"[player] … {name}"` also work.

## Arguments

`open:<menu>:<argument>` passes text to the menu. Use it as `<arg>` in text, `{arg}` in commands, the value `arg`, and the flag `has-arg`.

```yaml
"H":
  action: "open:player-card:{player}"
```

## Limiting who can open it

```yaml
staff-tools:
  permission: myserver.staff
  open-requirement: "op | perm:inkcontracts.admin"
  deny-message: "<red>Staff only."
```

Other plugins can add their own sources and menus through the [API](../developer-api).
