---
title: Storage
parent: Configuration
nav_order: 6
---

# Storage

Set in `config.yml` → `storage`. Changing the backend needs a **restart**.

| `backend` | Where | Good for |
|---|---|---|
| `json` (default) | `plugins/InkContractsPlus/data/` | Most servers. Needs no setup |
| `sqlite` | One file, `data/inkcontracts.db` | Larger single servers |
| `mysql` / `mariadb` | A database server | Large servers, or keeping data outside the server folder |

```yaml
storage:
  backend: json
  autosave-seconds: 120
  sqlite:
    file: data/inkcontracts.db
  mysql:
    host: localhost
    port: 3306
    database: minecraft
    username: root
    password: ""
    table-prefix: "ic_"
    parameters: "useSSL=false&allowPublicKeyRetrieval=true&characterEncoding=utf8"
    pool:
      maximum-pool-size: 6
      minimum-idle: 2
      connection-timeout-ms: 10000
      max-lifetime-ms: 1800000
```

SQL backends only write what changed (contracts, profiles and balances), so saves stay fast as the data grows.

## Switching backend

1. Set up the new backend's settings (for MySQL: host, database, user).
2. Run `/contract admin migrate <json|sqlite|mysql>`. This copies **all** data to the new backend.
3. Set `storage.backend` to the new backend.
4. Restart.

The old data is left where it was, as a backup.

## Libraries

The SQLite and MySQL drivers come with Paper. For SQLite and MySQL, Paper downloads the **HikariCP** connection pool from Maven Central once, on first start. With JSON storage nothing is downloaded.
