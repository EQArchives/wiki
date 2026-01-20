---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-20T06:02:03.651Z
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

## Tables That Should be Copied Forward
- character_alternate_abilities - AA progression
- character_bind - Bind points
- character_buffs - Active buffs
- character_currency - Money (platinum, gold, silver, copper)
- character_data - Core character info (name, level, class, race, stats, etc.)
- character_faction_values - Faction standings
- character_inventory - Items in inventory/equipped
- character_keyring - Keys collected
- character_languages - Language skills
- character_memmed_spells - Memorized spells
- character_skills - Skill values
- character_spells - Learned spells/scribed spells