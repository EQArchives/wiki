---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-25T20:49:13.882Z
tags: database
editor: markdown
dateCreated: 2026-01-20T03:13:49.975Z
---

# Character Migration
This article documents the schema compatibility between TAKP and PEQ database formats, and provides insight into what tables should be copied, and which ones aren't important.

>Database versioning would be a good future step, but not there yet.
{.is-warning}
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
| [character_alternate_abilities](character_alternate_abilities) | ✅ Yes | Primary key structure differs. Requires deduplication if same AA exists in multiple slots. Recommend checking for duplicates first. |
| [character_bind](character_bind) | ✅ Yes | High compatibility. Simple field rename (`is_home` → `slot`). |
| [character_currency](character_currency) | ✅ Yes | Excellent compatibility. All TAKP fields map 1:1 to PEQ. |
| [character_data](character_data) |  ✅ Yes | High compatibility. All core fields identical. PEQ expansion features set to 0. See [full comparison](/migration/tables/character-data) for field mappings. |
| [character_faction_values](character_faction_values) | ✅ Yes | Excellent compatibility. Field rename: `id`→`char_id`. Direct 1:1 copy of all faction standings. |
| [character_inventory](character_inventory) | ✅ Yes | Good compatibility. Table rename: `character_inventory`→`inventory`. Field renames: `id`→`character_id`, `slotid`→`slot_id`, `itemid`→`item_id`. Set augment/ornament fields to 0. TAKP serial tracking lost. |
| [character_keyring](/character_keyring) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_languages](character_languages) | ✅ Yes | Identical schemas. Direct 1:1 copy, no adjustments needed. |
| [character_memmed_spells](character_memmed_spells) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_skills](character_skills) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_spells](character_spells) | ⚠️ Pending | Schema comparison not yet complete. |
| [character_buffs](character_buffs) | ❌ No | Transient runtime data. Standard practice is to skip - characters rebuff naturally after login. |
| character_consent | ❌ No | Used for consenting players to drag corpses with /corpse |
| character_corpse_items | ❌ No | Items left on active corpses |
| character_corpse_items_backup | ❌ No | backup of items left on corpses |
| character_corpses | ❌ No | active corpses |
| character_corpses_backup | ❌ No | backup of corpses - usually after it decays |
| character_inspect_messages | ❌ No | Typically unused in TAKP |
| character_lookup | ❌ No | Typically unused in TAKP |
| character_magelo_stats | ❌ No | Does not exist in PEQ schema.  Used for TAKP magelo web app |
| character_pet_buffs | ❌ No | Temporary buffs on pets. |
| character_pet_info | ❌ No | Summoned character pets |
| character_pet_inventory | ❌ No | Equipment handed to character pets |
| character_soulmarks | ❌ No | 
| character_timers | ❌ No | Timers for characters (like spell recast timers) |
| character_zone_flags | ❌ No | Flags for whether a character is flagged for a zone (PoP) only |

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
## Ephemeral Character Tables
The following character-related tables are somewhat ephemeral, or specific to a server and do not need to be migrated forward.

* account_flags
* account_rewards
* friends
* guilds
* guild_ranks
* guild_members
* mail
* petitions
* player_titlesets
* quest_globals
* spell_globals
* client_version
* commands_log
* titles
* trader
