---
title: Permissions
nav_order: 5
---

# Permissions

| Permission | Default | Lets players… |
|---|---|---|
| `inkcontracts.use` | everyone | browse, accept and work on contracts |
| `inkcontracts.create` | everyone | post contracts |
| `inkcontracts.label` | everyone | post under a custom issuer name |
| `inkcontracts.templates` | everyone | use templates |
| `inkcontracts.wallet` | everyone | see their own balances |
| `inkcontracts.pay` | everyone | use `/contract pay` |
| `inkcontracts.currency.*` | everyone | post in any currency. One currency: `inkcontracts.currency.<id>` |
| `inkcontracts.wallet.others` | op | see other players' balances |
| `inkcontracts.notify.disputes` | op | get alerts when a dispute is opened |
| `inkcontracts.bypass.limits` | op | ignore the open/active contract limits |
| `inkcontracts.admin` | op | use every staff command. It includes all the permissions above except `currency.*` |

## Your own permissions

Several things take a permission name you choose:

| Where | Key | Controls |
|---|---|---|
| `types.yml` | `permission` | who may **post** this type |
| `types.yml` | `accept-permission` | who may **accept** this type |
| `templates.yml` | `permission` | who may use this template |
| `guis.yml` (menu) | `permission` | who may open the menu |
| `guis.yml` (button) | `permission` | who sees and may click the button |

Example: VIP-only Construction contracts.

```yaml
# types.yml
types:
  CONSTRUCTION:
    permission: "myserver.vip"
```

## Limiting currencies

To stop a group using a currency, negate `inkcontracts.currency.*` for that group and grant the currencies they may use, e.g. `inkcontracts.currency.credits`. Players only see the currencies they're allowed to use.
