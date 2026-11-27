---
title: Items and buttons
parent: Menus
nav_order: 1
---

# Items and buttons
{: .no_toc }

1. TOC
{:toc}

## Items

Every item in `guis.yml`, and every `icon:` in the other files, accepts:

| Key | |
|---|---|
| `material` | Any item id: `DIAMOND`, `minecraft:diamond` |
| `amount` | A number, or a live value such as `stash`, `active` or `board` |
| `name` | MiniMessage |
| `lore` | List of lines, or one string |
| `wrap` | Override `settings.lore-wrap` for this item (0 = off) |
| `glow` | Enchantment glint |
| `hide-tooltip` | No tooltip at all (good for glass panes) |
| `item-model` | Resource-pack model, e.g. `myserver:coin` |
| `custom-model-data` | Number, for older resource packs |
| `tooltip-style` | Resource-pack tooltip style |
| `skull-owner` | Player name, e.g. `"{player}"` for the viewer |
| `skull-texture` | Base64 texture value |
| `preset` | Start from a [preset](reuse) |

Long lore lines are wrapped automatically, keeping their colours. A lore line that's empty after its [conditions](conditions) are applied is removed, so hidden lines don't leave gaps. A line that's empty in the config (`""`) is always a spacer. A line that's only a condition, e.g. `"[if:click]"`, is a spacer only while that condition is true.

### Animation

```yaml
"L":
  material: LANTERN
  frames:
    - { material: SOUL_LANTERN }
    - { material: LANTERN, glow: true }
  frame-ticks: 10
```

`frames` cycle every `frame-ticks` ticks. Each frame is a full item, and missing keys come from the item itself.

## Placing items

* **By layout character:** the item's key is one character that appears in `layout`.
* **By slot:** any longer key, with `slots:`:

```yaml
items:
  corners:
    slots: [0, 8, "45-53"]
    preset: accent
```

### Candidates

A slot can hold a **list**. The first entry that's visible (its `view-requirement` passes) and usable is shown. This is how the contract page shows *Accept*, *Deliver*, *Submit* or *Approve* in the same slot:

```yaml
"N":
  - action: draft                       # only usable when you have a draft
    material: WRITABLE_BOOK
    name: "<green>Continue your draft"
  - action: create
    material: WRITABLE_BOOK
    name: "<green>Post a contract"
```

## Clicks

The shortest form is a single action for any click:

```yaml
action: next-page
```

Or one per click type:

```yaml
click:
  left: next-page
  shift-left: last-page
  right: [ "open:wallet", "[sound] minecraft:ui.button.click" ]
```

The click types are `left`, `right`, `shift-left`, `shift-right`, `middle`, `drop`, `control-drop`, `number-key`, `double`, `swap-offhand` and `any`.

A click type with nothing set falls back to a broader one: `shift-left` → `left` → `any`, `control-drop` → `drop` → `any`, and so on.

The older keys `action:`, `right-action:` and `commands:` still work.

## Steps

Each click runs one **step** or a list of them. A step is either an **action** or a **command**.

### Actions

Available in every menu:

| Action | |
|---|---|
| `close`, `back`, `refresh` | |
| `previous-page`, `next-page`, `first-page`, `last-page` | |
| `open:<menu>` / `open:<menu>:<argument>` | Open a menu, optionally with an [argument](custom-menus#arguments) |
| `create` | The creation wizard, or the player's draft |
| `draft` | The player's draft |
| `view:<id>` | Open a contract |
| `none` | Do nothing (decoration) |

Menu-specific actions:

| Menu | Actions |
|---|---|
| `board` | `filter`, `filter-previous`, `currency`, `currency-previous`, `sort`, `sort-previous`, `search`, `clear-search`, `toggle-own`, `reset-filters`, `profile` |
| `contracts` | `toggle-finished`, `sort`, `sort-previous` |
| `contract` | `accept`, `deliver`, `hand-in`, `submit`, `abandon`, `approve`, `reject`, `cancel`, `dispute`, `rate-1` … `rate-5`, `reward-items`, `region`, `destination`, `resolve`, `issuer`, `contractor`; plus `card` and `timeline` to show the contract itself |
| `stash` | `claim-all` |

Other plugins can add actions through the [API](../developer-api).

### Commands

| Step | |
|---|---|
| `[player] <command>` | Run as the player (no `/`) |
| `[console] <command>` | Run from the console |
| `[message] <text>` | Chat message (MiniMessage) |
| `[actionbar] <text>` | Action bar |
| `[broadcast] <text>` | Message to everyone |
| `[sound] <key> [volume] [pitch]` | Play a sound |
| `[close]`, `[refresh]`, `[back]` | |
| `[open] <menu>` | Same as `open:` |
| `[action] <name>` | Run an action |

Tokens: `{player}`, `{uuid}`, `{page}` and `{arg}`. Placeholders and PlaceholderAPI work in commands too.

## Rules

```yaml
"D":
  action: dispute
  material: BELL
  name: "<red>Open a dispute"
  permission: inkcontracts.use
  view-requirement: "active | submitted"       # hidden otherwise
  click-requirement: "!disputed"               # visible, but clicking is refused…
  deny-message: "<red>Already disputed."       # …with this message
  confirm: { title: "Open a dispute?", text: "Staff will review this contract.", yes: "Dispute" }
  cooldown: 5s
  sound: menu-click
  deny-sound: menu-deny
  unavailable: { preset: border }              # look when it can't be used
  hide-when-unavailable: false
```

| Key | |
|---|---|
| `permission` | Hidden unless the player has it |
| `view-requirement` | [Condition](conditions). Hidden unless it's true |
| `click-requirement` + `deny-message` | Condition checked on click |
| `confirm` | `true`, `"text"`, or `{ title, text, yes }`. Asks with a dialog before running |
| `cooldown` | `500ms`, `3s`, `1m` or `40t` (ticks) |
| `sound` / `deny-sound` | Sound name from `config.yml` or a raw key |
| `unavailable` | Item to show when the action can't be used right now, e.g. "next page" on the last page |
| `hide-when-unavailable` | `true` leaves the slot empty instead |
