---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-25T15:24:36.021Z
tags: database
editor: markdown
dateCreated: 2026-01-20T03:13:49.975Z
---

# Character Migration

## Account Tables

| Table Name | Copy? | Notes |
| --- | --- | --- |
| [account](account) | ✅ Yes | Good compatibility. Field rename: `gminvul`→`invulnerable`. Set `ls_id` to 'local', `auto_login_charname` to empty. TAKP fields lost: `forum_id`, `expansion`, `active`, `ip_exemption_multiplier`, `mule`. |
| [account_ip](account_ip) | ✅ Yes | Identical schemas. Direct 1:1 copy, no adjustments needed. |
| account_flags | ❌ No | Typically unused in TAKP. |
| account_rewards | ❌ No | Typically unused in TAKP. |

## Character Tables
| Table Name | Copy? | Notes |
| --- | --- | --- |
| [character_alternate_abilities](character_alternate_abilities) | ⚠️ Conditional | Primary key structure differs. Requires deduplication if same AA exists in multiple slots. Recommend checking for duplicates first. |
| [character_bind](character_bind) | ✅ Yes | High compatibility. Simple field rename (`is_home` → `slot`). |
| [character_buffs](character_buffs) | ❌ No | Transient runtime data. Standard practice is to skip - characters rebuff naturally after login. |
| [character_currency](character_currency) | ✅ Yes | Excellent compatibility. All TAKP fields map 1:1 to PEQ. |
| [character_data](character_data) |  ✅ Yes | High compatibility. All core fields identical. PEQ expansion features set to 0. See [full comparison](/migration/tables/character-data) for field mappings. |
| [character_faction_values](character_faction_values) | ✅ Yes | Excellent compatibility. Field rename: `id`→`char_id`. Direct 1:1 copy of all faction standings. || [character_inventory](character_inventory) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_keyring](/character_keyring) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_languages](character_languages) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_memmed_spells](character_memmed_spells) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_skills](character_skills) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_spells](character_spells) | ⚠️ Pending | Schema comparison not yet complete. |

## login_accounts
### PEQ Format
```sql
+--------------------+------------------+------+-----+---------------------+-------+
| Field              | Type             | Null | Key | Default             | Extra |
+--------------------+------------------+------+-----+---------------------+-------+
| id                 | int(11) unsigned | NO   | PRI | NULL                |       |
| account_name       | varchar(50)      | NO   |     | NULL                |       |
| account_password   | text             | NO   |     | NULL                |       |
| account_email      | varchar(100)     | NO   |     | NULL                |       |
| source_loginserver | varchar(64)      | YES  | MUL | NULL                |       |
| last_ip_address    | varchar(80)      | NO   |     | NULL                |       |
| last_login_date    | datetime         | NO   |     | NULL                |       |
| created_at         | datetime         | YES  |     | NULL                |       |
| updated_at         | datetime         | YES  |     | current_timestamp() |       |
+--------------------+------------------+------+-----+---------------------+-------+
9 rows in set (0.001 sec)
```

