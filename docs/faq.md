---
title: FAQ
nav_order: 10
---

# FAQ
{: .no_toc }

1. TOC
{:toc}

## The console says "Currency 'money' (vault) could not be loaded"

Vault isn't installed, so the plugin uses the built-in `credits` currency instead. If you don't want Vault, turn `money` off and make credits the default. See [No economy plugin?](configuration/currencies#no-economy-plugin)

## Players say their mining doesn't count

Only blocks from the natural world count. Blocks a player placed, including ones moved by pistons, never count, even if someone else placed them. For Gathering, the items must also be handed in with `/contract deliver`, unless you set `require-delivery: false` for the type.

## Kills don't count

Mobs from spawners, trial spawners, spawn eggs, commands and plugins don't count, and neither do built golems. If your mobs come from a plugin that spawns them as `CUSTOM`, remove `CUSTOM` from `verification.hunting-ignored-spawn-reasons`. That plugin can also credit kills itself through the [API](developer-api#your-own-progress).

## A player was offline when they got paid

Their items wait in the stash (`/contract claim`), their messages are shown when they join, and money that couldn't be paid is held as *pending* and paid automatically.

## How do I stop players posting a certain item or mob?

Use `blocked-targets` on the type in [types.yml](configuration/types), e.g. `blocked-targets: ["*_spawn_egg", "minecraft:elytra"]`. Use `allowed-targets` to allow only certain ones.

## How do I make posting cost money?

Raise `listing.fee-flat` / `fee-percent`. Use `completion-tax-percent` to also take a cut of payouts. Both can be set per currency and per type.

## I broke a menu

Run `/contract admin menus`. It tells you the menu, the button and what's wrong. To go back to the default for one menu, delete its section from `guis.yml` and run `/contract admin reload`.

## A menu I deleted came back

Bundled menus are added back when they're missing. Set them to `false` to turn them off ([details](menus/#turning-menus-off-and-resetting-them)).

## Does it work on Spigot or Folia?

No. It's a Paper plugin, written for Paper 26.3 and its forks such as Purpur. It isn't Folia-compatible.

## Does it phone home or download anything?

Nothing is downloaded with the default JSON storage. With SQLite or MySQL, Paper downloads the HikariCP library from Maven Central once. No data is ever sent anywhere.

## Can I use it as my server's only economy?

Yes. Set the `credits` currency's `vault-provider: true` and other plugins (shops, jobs…) will use it through Vault. Vault itself must be installed, and no other economy plugin should be.

## How do I move from JSON to MySQL?

See [Storage → Switching backend](configuration/storage#switching-backend).
