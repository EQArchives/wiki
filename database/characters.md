---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-20T03:13:49.975Z
tags: database
editor: markdown
dateCreated: 2026-01-20T03:13:49.975Z
---

# Character Migration

## Select TAKP account for insertion to EQEMU account schema
The following query grabs TAKP account data and adds additional fields left as blank for eqemu.
```sql
SELECT 
    id,
    name,
    charname,
    '',                   -- auto_login_charname
    sharedplat,
    password,
    status,
    'local',              -- ls_id
    lsaccount_id,
    gmspeed,
    gminvul,              -- maps to invulnerable
    flymode,
    ignore_tells,
    revoked,
    karma,
    minilogin_ip,
    hideme,
    rulesflag,
    NULL,                 -- suspendeduntil (always NULL)
    time_creation,
    ban_reason,
    suspend_reason,
    NULL,                 -- crc_eqgame
    NULL,                 -- crc_skillcaps
    NULL                  -- crc_basedata
FROM account
WHERE name LIKE "soandso%";
```