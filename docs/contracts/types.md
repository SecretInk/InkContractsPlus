---
title: Contract types
parent: Contracts
nav_order: 1
---

# Contract types
{: .no_toc }

1. TOC
{:toc}

| Type | What the contractor does | How it's verified |
|---|---|---|
| **Delivery** | Hands in items | Items are taken from the inventory and sent to the issuer's stash |
| **Gathering** | Mines blocks, then hands in the drops | Only drops from **natural** blocks the contractor broke count |
| **Hunting** | Kills mobs | Kill credit goes to the killer. Spawner, spawn-egg, command and golem mobs don't count |
| **Farming** | Harvests crops, then hands in the produce | Only fully grown crops and naturally grown melons, pumpkins, cane… |
| **Crafting** | Crafts items, then hands them in | Crafting is tracked |
| **Fishing** | Fishes items up, then hands them in | Only real catches |
| **Construction** | Builds inside a marked region | Blocks are counted on submit, then the issuer signs off |
| **Transport** | Carries a sealed package to a destination | Position check. The courier posts collateral |
| **Assistance** | Helps with a described task | The issuer confirms. This is the only type the plugin doesn't check |

Every type can be renamed, restyled, restricted or switched off in [types.yml](../configuration/types).

## Gathering, Farming, Crafting, Fishing

The contractor first does the verified action, then hands the items in, which moves them to the issuer's stash. Progress shows two numbers: how many were **verified** and how many were **delivered**.

To make the verified action alone complete the objective, set `require-delivery: false` on the type in `types.yml`. The items then stay with the contractor.

**Placed blocks never count.** Every block a player places is remembered per chunk, and the record moves with the block when a piston pushes it. Placing a block and mining it again gives no progress. This is controlled by `verification.ignore-placed-blocks` in `config.yml`.

**Farming** counts fully grown crops and berry harvests. Non-ageable produce (melon, pumpkin, sugar cane, cactus, bamboo, kelp) only counts when it grew naturally. The list is `verification.farm-extra-blocks`.

## Hunting

The player who lands the kill gets the credit. Mobs that spawned for these reasons never count: spawners, trial spawners, spawn eggs (used or dispensed), thrown eggs, built golems, snowmen and withers, plugins (`CUSTOM`) and commands. The list is `verification.hunting-ignored-spawn-reasons`.

By default, the Ender Dragon and the Wither can't be hunting targets (`blocked-targets` in `types.yml`).

## Construction

The issuer marks a region with `/contract draft pos1` and `/contract draft pos2` while standing at two corners, then lists the blocks they want (e.g. `stone_bricks 400`). The region can't be bigger than `construction.max-volume` blocks.

When the contractor runs `/contract submit <id>`, the plugin counts the blocks inside the region.
* With `construction.require-issuer-review: true` (the default), a successful count sends the contract to the issuer for review. If they don't respond within `contracts.review-hours`, it is approved automatically.
* With `false`, a successful count completes the contract immediately.

## Transport

The issuer packs items into a **package** (`/contract draft package`) and sets the **destination** to where they're standing (`/contract draft destination`). To have the goods delivered into a chest, stand on the chest's block position, e.g. place the chest after setting the destination.

When a courier accepts:
1. The courier pays **collateral**: `transport.collateral-percent` of the reward, in the contract's currency.
2. They receive the package items, which are tagged so they can't be swapped for others.
3. They must reach within `transport.delivery-radius` blocks of the destination and deliver. If there's a chest or other container at the destination, the goods go into it. Anything that doesn't fit goes to the issuer's stash.

If the package isn't delivered, the collateral goes to the issuer.

## Assistance

Free-text tasks ("help me build for an hour"). The contractor submits and the issuer approves or rejects, with the same auto-approval timer as Construction.

Because the plugin can't check this work, the type has `min-reputation: 0` by default, so players with negative reputation can't accept it.
