---
title: templates.yml
parent: Configuration
nav_order: 4
---

# templates.yml

Templates are ready-made contracts. Players open `/contract templates`, or run `/contract template <id>`, to start a pre-filled draft. They can change anything before publishing.

```yaml
templates:
  diamond_run:
    type: GATHERING
    title: "Diamond run"
    label: ""                        # optional issuer label
    icon: { material: DIAMOND_ORE, glow: true }
    description:
      - "<gray>Mine 16 diamonds from natural ore."
    reward: 900
    currency: money                  # missing = the player's choice / default
    deadline-hours: 72
    min-reputation: 10
    permission: ""                   # "" = anyone with inkcontracts.templates
    objectives: ["minecraft:diamond 16"]
```

| Key | |
|---|---|
| `type` | A contract type |
| `title`, `label` | Defaults for the draft |
| `icon` | Menu item. Missing = the type's icon |
| `description` | Lore lines in the templates menu |
| `reward`, `currency`, `deadline-hours`, `min-reputation` | Defaults for the draft |
| `permission` | Your own permission name |
| `objectives` | `"<id> <amount>"`: items for Delivery, Gathering, Farming, Crafting and Fishing; mobs for Hunting; blocks for Construction |
| `task` | Text for Assistance |

The bundled templates are `stone_bulk`, `diamond_run`, `zombie_cull`, `wheat_harvest`, `fishing_trip` and `help_build`. Delete them or add your own; deleted templates stay deleted when the plugin updates.

Templates a player can't use (missing permission, disabled type, or a currency they can't use) don't appear in the menu.
