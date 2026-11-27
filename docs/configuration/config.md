---
title: config.yml
parent: Configuration
nav_order: 1
---

# config.yml
{: .no_toc }

1. TOC
{:toc}

## listing

Rules for posting. `min-reward`, `max-reward`, `fee-flat`, `fee-percent` and `completion-tax-percent` are global defaults; each [currency](currencies#limits) and [type](types) can override them.

| Key | Default | |
|---|---|---|
| `min-reward` / `max-reward` | 10 / 1000000 | Reward range |
| `fee-flat` / `fee-percent` | 0 / 2.0 | Listing fee = max(flat, reward × percent / 100). Never refunded |
| `completion-tax-percent` | 0 | Taken from the contractor's payout |
| `min-deadline-hours` / `max-deadline-hours` | 1 / 336 | Deadline range |
| `default-deadline-hours` | 48 | |
| `max-open-per-player` | 5 | Open contracts a player may have posted at once |
| `max-objectives` | 6 | Objectives per contract |
| `max-amount-per-objective` | 100000 | |
| `title-max-length` / `label-max-length` | 32 / 32 | |
| `broadcast-new` | true | Announce new contracts to everyone online |
| `reward-items.enabled` | true | Allow bonus item rewards |
| `reward-items.max-stacks` | 9 | |
| `reward-items.blacklist` | bedrock, command block, barrier | Items that can't be rewards |

## contracts

| Key | Default | |
|---|---|---|
| `max-active-per-player` | 3 | Contracts a player may work on at once |
| `partial-payout` | true | Pay for delivered goods when a contract fails, is abandoned or is cancelled |
| `abandon-cooldown-minutes` | 30 | Wait after abandoning before accepting another contract |
| `review-hours` | 24 | Submitted work is approved automatically after this long |
| `issuer-cancel-active` | true | May issuers cancel work in progress? |
| `issuer-cancel-compensation-percent` | 10 | Paid to the contractor from the remaining escrow when that happens |
| `prune-after-days` | 30 | Delete finished contracts from storage after this long (stats are kept) |
| `rating-window-days` | 7 | How long both sides can rate each other |

## verification

| Key | |
|---|---|
| `ignore-placed-blocks` | Player-placed blocks never count as gathered (tracked per chunk, piston-aware) |
| `hunting-ignored-spawn-reasons` | Spawn reasons whose mobs never count: `SPAWNER`, `TRIAL_SPAWNER`, `SPAWNER_EGG`, `DISPENSE_EGG`, `EGG`, `BUILD_*`, `CUSTOM`, `COMMAND` |
| `farm-extra-blocks` | Non-crop blocks that count as farm produce when grown naturally |

## construction

| Key | Default | |
|---|---|---|
| `max-volume` | 125000 | Largest region in blocks (dx × dy × dz) |
| `require-issuer-review` | true | `false`: a successful scan completes the contract immediately |

## transport

| Key | Default | |
|---|---|---|
| `collateral-percent` | 50 | Courier's deposit, as a percent of the reward |
| `delivery-radius` | 6 | How close to the destination the courier must be |

## reputation

The weights and tiers are explained on [Reputation](../contracts/reputation).

## notifications

| Key | Default | |
|---|---|---|
| `progress` | `actionbar` | How progress is shown while working: `actionbar`, `bossbar`, `both` or `none` |
| `bossbar-seconds` | 6 | How long the boss bar stays after the last progress |
| `bossbar-color` / `bossbar-overlay` | `YELLOW` / `NOTCHED_10` | |
| `throttle-ms` | 400 | Minimum time between progress updates (milestones always show) |
| `title-fade-in` / `title-stay` / `title-fade-out` | 10 / 50 / 15 | Ticks, for `[title]` messages |

## sounds

Format: `"<sound key> [volume] [pitch]"`. An empty string means silent. Any vanilla or resource-pack sound works.

`menu-click`, `menu-open`, `menu-deny`, `menu-page`, `menu-toggle`, `menu-close`, `publish`, `accept`, `progress`, `objective-done`, `deliver`, `submit`, `complete`, `fail`, `dispute`, `payment`, `error`.

Menu buttons can use these names, e.g. `sound: menu-page`, or a raw key.

## storage

See [Storage](storage).
