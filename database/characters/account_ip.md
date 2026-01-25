---
title: account_ip
description: 
published: true
date: 2026-01-25T02:04:27.851Z
tags: database, takp_peq_migration, account
editor: markdown
dateCreated: 2026-01-25T02:04:27.851Z
---

## account_ip

> **ℹ️ Schema Compatibility**
> 
> The `account_ip` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### TAKP Format
```sql
+-----------+-------------+------+-----+---------------------+-------+
| Field     | Type        | Null | Key | Default             | Extra |
+-----------+-------------+------+-----+---------------------+-------+
| accid     | int(11)     | NO   | PRI | 0                   |       |
| ip        | varchar(32) | NO   | PRI | NULL                |       |
| count     | int(11)     | NO   |     | 1                   |       |
| lastused  | timestamp   | NO   |     | current_timestamp() |       |
+-----------+-------------+------+-----+---------------------+-------+
```

### PEQ Format
Same as TAKP (see above)