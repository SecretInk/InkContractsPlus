---
title: Developer API
nav_order: 9
---

# Developer API
{: .no_toc }

1. TOC
{:toc}

## Setup

Add the InkContracts+ jar to your project as a `compileOnly` dependency, e.g. from a local `libs/` folder:

```kotlin
dependencies {
    compileOnly(files("libs/InkContractsPlus-1.0.0.jar"))
}
```

Declare the dependency in your `paper-plugin.yml` (or `plugin.yml`):

```yaml
dependencies:
  server:
    InkContractsPlus:
      load: BEFORE
      required: false        # true if your plugin can't work without it
      join-classpath: true
```

Get the API from Bukkit's services manager:

```java
InkContractsAPI api = Bukkit.getServicesManager().load(InkContractsAPI.class);
if (api == null) return; // InkContracts+ isn't installed
```

## InkContractsAPI

| Method | |
|---|---|
| `currency(id)`, `currencies()`, `defaultCurrency()` | Look up currencies |
| `registerCurrencyType(type, factory)` | Add a currency type for `currencies.yml` |
| `contract(id)`, `openContracts()`, `activeContracts(uuid)` | Read contracts |
| `reputation(uuid)`, `tier(uuid)` | Reputation score and tier name |
| `credit(player, kind, target, amount)` | Give verified progress from your own plugin (e.g. a custom mob or ore) |
| `payout(uuid, currency, amount)` | Pay a player. If they can't be paid right now, the amount is kept as pending |
| `menus()` | The [menu registry](#menus) |
| `openMenu(player, "name[:argument]")` | Open any menu. Returns false if it doesn't exist |

### Your own progress

```java
// e.g. a custom-mobs plugin crediting a kill
api.credit(player, ObjectiveKind.KILL, NamespacedKey.minecraft("zombie"), 1);
```

`ObjectiveKind` values are `DELIVER`, `GATHER`, `FARM`, `CRAFT`, `FISH`, `KILL`, `BUILD` and `TASK`. Credited progress goes through `ContractProgressEvent` like built-in progress.

## Currency types

```java
api.registerCurrencyType("tokens", (id, section) -> new TokenCurrency(id, section));
```

```yaml
# currencies.yml
currencies:
  tokens:
    type: tokens
    display-name: "<gold>Tokens"
```

Implement `Currency`. Its main methods are:
* `balance`, `withdraw`, `deposit` (all take an `OfflinePlayer`)
* `format`, `decimals`, `available`, `offlineWithdraw`, `icon`, `limits`

`CurrencyLimits.from(section.getConfigurationSection("limits"))` reads the standard limits.

## Menus

`api.menus()` is the registry the built-in menus use. Anything registered there can be used straight from `guis.yml`:

```java
MenuRegistry menus = api.menus();

// action: daily-bonus      (also "daily-bonus:<argument>")
menus.registerAction("daily-bonus", (menu, arg, click) -> giveBonus(menu.viewer()));

// [if:vip]   view-requirement: "vip"
menus.registerFlag("vip", (player, arg) -> player.hasPermission("rank.vip"));

// [if:kills >= 100]   amount: kills
menus.registerValue("kills", (player, arg) -> String.valueOf(player.getStatistic(Statistic.MOB_KILLS)));

// <rank>  or  <rank:argument>  (the result is MiniMessage)
menus.registerPlaceholder("rank", (player, arg) -> "<gold>" + rankOf(player));

// source: bounties
menus.registerSource("bounties", (menu, options) -> bounties().stream()
        .map(b -> new ConfiguredMenu.Entry(b.icon(), e -> claim(menu.viewer(), b)))
        .toList());

// open:bank   /contract menu bank
menus.registerMenu("bank", (player, back, arg) -> new BankMenu(player, back));
```

An `Action` can also override `available(menu, arg)`. When it returns false, the button shows its `unavailable:` look.

A menu id with both a registered factory and a section in `guis.yml` gets the factory. To make a Java menu whose layout, buttons and conditions come from `guis.yml`, extend `ConfiguredMenu`.

## Events

All events are in `com.inkspot.inkContractsPlus.api.event`.

| Event | Cancellable | |
|---|---|---|
| `ContractPublishEvent` | ✔ | `contract()`, `issuer()` |
| `ContractAcceptEvent` | ✔ | `contract()`, `contractor()` |
| `ContractProgressEvent` | ✔ | `contract()`, `player()`, `objective()`, `amount()`; `amount(int)` changes the credited amount |
| `ContractStatusChangeEvent` | | `contract()`, `from()`, `to()` |
| `ContractCompleteEvent` | | `contract()`, `contractor()`, `currency()`, `paid()` |
| `MenuOpenEvent` | ✔ | `player()`, `menu()` |
| `MenuClickEvent` | ✔ | `player()`, `menu()`, `button()` (`<menu>:<key>`, plus `#<n>` for a candidate in a list), `slot()`, `click()`; `steps()` is the mutable list of steps about to run |

```java
@EventHandler
public void onComplete(ContractCompleteEvent e) {
    if (e.paid() >= 10_000) Bukkit.broadcast(Component.text("A big contract was just completed!"));
}

@EventHandler
public void onMenuClick(MenuClickEvent e) {
    if (e.button().equals("main:T")) e.steps().add("[sound] minecraft:entity.player.levelup");
}
```

{: .note }
Events fire on the server thread. Don't block in handlers.
