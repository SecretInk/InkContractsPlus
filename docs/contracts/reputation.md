---
title: Reputation and ratings
parent: Contracts
nav_order: 4
---

# Reputation and ratings

## Ratings

After a contract completes, both sides get a clickable ★★★★★ prompt in chat. They can also rate with `/contract rate <id> <1-5>` within `contracts.rating-window-days` days.

* A **contractor's** rating is shown as their *rating*.
* An **issuer's** rating is shown as their *issuer rating*.

## Reputation score

Reputation is a weighted sum. The weights are set in `config.yml` → `reputation.weights`:

| Event | Default points |
|---|---|
| Completed a contract | +10 |
| Failed (missed the deadline) | −20 |
| Abandoned | −8 |
| One of your contracts was completed (as issuer) | +3 |
| Cancelled someone's active work | −10 |
| Lost a dispute | −15 |
| Rating | `rating` × (average stars − 3) × min(number of ratings, 10) |

With the default `rating: 2`, a player averaging 4.5★ over 10 or more ratings gets +30.

## Tiers

The highest tier whose `min` the player reaches is shown next to their name on contracts and in menus. Names use MiniMessage.

```yaml
reputation:
  tiers:
    - { min: -1000000, name: "<dark_red>Blacklisted" }
    - { min: -40, name: "<red>Unreliable" }
    - { min: 0, name: "<gray>Newcomer" }
    - { min: 30, name: "<green>Reliable" }
    - { min: 100, name: "<aqua>Trusted" }
    - { min: 250, name: "<light_purple>Veteran" }
    - { min: 500, name: "<gold>Elite" }
```

## Minimum reputation

Issuers can require a minimum reputation to accept their contract. Each type can also have its own `min-reputation` in `types.yml`. For example, Assistance requires 0 by default, which keeps players with negative reputation away from unchecked work.

## Seeing reputation

* `/contract rep [player]`
* `/contract history [player]`
* the profile head in the menus
* the leaderboard: `/contract top`
* placeholders: `%inkcontracts_reputation%`, `%inkcontracts_tier%`
