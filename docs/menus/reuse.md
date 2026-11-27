---
title: Presets and inheritance
parent: Menus
nav_order: 3
---

# Presets and inheritance

## Presets

A preset is a named item you can reuse. Define it once under `presets:`:

```yaml
presets:
  next:
    click:
      left: next-page
      shift-left: last-page
    sound: menu-page
    material: SPECTRAL_ARROW
    name: "<#f5c542>Next page ›"
    unavailable: { preset: border }
```

Use it anywhere, and override any keys:

```yaml
items:
  ">": { preset: next }
  "N": { preset: next, material: ARROW, name: "<white>More" }
```

* **Presets of presets:** a preset can use another preset (up to 8 levels deep).
* **Several presets:** `preset: [nav, glowing]` applies them in order.
* **Fallback:** a preset missing from your file falls back to the bundled one.

The bundled presets are `filler`, `border`, `accent`, `back`, `home`, `close`, `previous`, `next` and `profile-head`. Change `border` once and every menu's border changes.

## extends

`extends:` starts a menu from another menu or from a **base**, then applies your keys on top. Sections merge key by key; lists such as `layout` and `lore` are replaced as a whole.

```yaml
bases:                       # abstract menus, never opened directly
  paged-list:
    layout:
      - "CCCCCCCCC"
      - "CCCCCCCCC"
      - "B<#####>X"
    content: "C"
    fill: { preset: border }
    items:
      "B": { preset: home }
      "<": { preset: previous }
      ">": { preset: next }
      "X": { preset: close }

menus:
  hunting-board:
    extends: paged-list
    title: "<red>Hunting jobs"
    source: contracts
    source-options: { type: HUNTING }
```

A menu can extend another menu too. The bundled `leaderboard-completed` is only:

```yaml
leaderboard-completed:
  extends: leaderboard
  title: "<dark_gray>Top contractors · <#8f6a00>completed"
  source-options: { stat: completed }
```
