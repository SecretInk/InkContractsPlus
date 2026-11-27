---
title: types.yml
parent: Configuration
nav_order: 3
---

# types.yml

The nine types are built in, because their verification is code. Everything about how they look and what rules apply is set here.

```yaml
types:
  HUNTING:
    enabled: true
    display-name: "<red>Hunting"
    description:
      - "<gray>Kill mobs. Spawner and"
      - "<gray>spawn-egg mobs don't count."
    icon: { material: IRON_SWORD }
    permission: ""                 # needed to POST this type ("" = anyone with inkcontracts.create)
    accept-permission: ""          # needed to ACCEPT it ("" = everyone)
    blocked-targets: ["minecraft:ender_dragon", "minecraft:wither"]
```

| Key | |
|---|---|
| `enabled` | `false` hides the type everywhere. Existing contracts still finish |
| `display-name` | MiniMessage |
| `description` | Lines shown when picking a type |
| `icon` | Any [item option](../menus/items-and-buttons#items). Also the default look of these contracts in lists |
| `permission` / `accept-permission` | Your own permission names |
| `require-delivery` | Gathering, Farming, Crafting and Fishing: `false` means the verified action alone completes an objective, with no hand-in |
| `min-reward`, `max-reward`, `fee-percent`, `fee-flat`, `max-objectives`, `default-deadline-hours`, `min-reputation` | Overrides for this type. `-1` or missing = use the currency's value, then `config.yml` |
| `currencies` | Allowed currency ids, e.g. `[money, credits]`. `[]` = all |
| `allowed-targets` / `blocked-targets` | Item, mob or block ids that may or may not be objectives. Wildcards work: `"minecraft:*_ore"`, `"*_spawn_egg"` |

## Examples

**Only ores for Gathering:**

```yaml
GATHERING:
  allowed-targets: ["minecraft:*_ore", "minecraft:ancient_debris", "minecraft:raw_*"]
```

**Transport only in real money (item currencies make poor collateral):**

```yaml
TRANSPORT:
  currencies: [money]
```

**No Assistance contracts:**

```yaml
ASSISTANCE:
  enabled: false
```
