---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-25T15:21:34.942Z
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

## character_buffs
Since character buffs are very temporary and not critical to character migration to a new server, I recommend not copying this table over and leaving it alone.

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (5 fields)</h4>
    <ul>
      <li><code>numhits</code></li>
      <li><code>dot_rune</code></li>
      <li><code>caston_x</code></li>
      <li><code>caston_y</code></li>
      <li><code>caston_z</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (1 field)</h4>
    <ul>
      <li><code>bufftype</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>9 identical fields</li>
      <li>3 fields with differences</li>
      <li>Total: 12 mappable fields</li>
    </ul>
  </div>
</div>

<div class="schema-legend">
  <div class="legend-item">
    <div class="legend-box" style="background-color: #e8f5e9;"></div>
    <span>Identical fields</span>
  </div>
  <div class="legend-item">
    <div class="legend-box" style="background-color: #fff3e0;"></div>
    <span>Compatible with differences</span>
  </div>
  <div class="legend-item">
    <div class="legend-box" style="background-color: #e3f2fd;"></div>
    <span>Exclusive to one schema</span>
  </div>
</div>

<table class="schema-comparison">
<thead>
  <tr>
    <th>Field Name</th>
    <th>PEQ Type</th>
    <th>PEQ Default</th>
    <th>TAKP Type</th>
    <th>TAKP Default</th>
    <th>Migration Notes</th>
  </tr>
</thead>
<tbody>
  <tr class="comparison-diff">
    <td><code>character_id</code> / <code>id</code></td>
    <td>int(10) unsigned PRI</td>
    <td>—</td>
    <td>int(10) PRI</td>
    <td>0</td>
    <td>Renamed: <code>id</code> (TAKP) → <code>character_id</code> (PEQ). Type differs: unsigned vs signed</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>slot_id</code></td>
    <td>tinyint(3) unsigned PRI</td>
    <td>—</td>
    <td>tinyint(3) unsigned PRI</td>
    <td>—</td>
    <td>Identical - buff slot position</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>spell_id</code></td>
    <td>smallint(10) unsigned</td>
    <td>—</td>
    <td>smallint(10) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>caster_level</code></td>
    <td>tinyint(3) unsigned</td>
    <td>—</td>
    <td>tinyint(3) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>caster_name</code></td>
    <td>varchar(64)</td>
    <td>—</td>
    <td>varchar(64)</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>ticsremaining</code></td>
    <td>int(11)</td>
    <td>—</td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>Type differs: int(11) signed vs int(10) unsigned</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>counters</code></td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>numhits</code></td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (hit-based buff tracking)</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>melee_rune</code></td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>magic_rune</code></td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>int(10) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>persistent</code></td>
    <td>tinyint(3) unsigned</td>
    <td>—</td>
    <td>tinyint(3) unsigned</td>
    <td>—</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>dot_rune</code></td>
    <td>int(10)</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (DoT damage absorption)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>caston_x</code></td>
    <td>int(10)</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (location where buff was cast)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>caston_y</code></td>
    <td>int(10)</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (location where buff was cast)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>caston_z</code></td>
    <td>int(10)</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (location where buff was cast)</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>ExtraDIChance</code></td>
    <td>int(10)</td>
    <td>0</td>
    <td>int(10)</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>instrument_mod</code> / <code>bard_modifier</code></td>
    <td>int(10)</td>
    <td>10</td>
    <td>tinyint(3) unsigned</td>
    <td>10</td>
    <td>Renamed: <code>bard_modifier</code> (TAKP) → <code>instrument_mod</code> (PEQ). Type differs but compatible</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>bufftype</code></td>
    <td>—</td>
    <td>—</td>
    <td>int(11)</td>
    <td>—</td>
    <td><strong>TAKP only:</strong> Lost on migration (purpose unknown without documentation)</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **⚠️ Temporary Data Warning**
> 
> The `character_buffs` table contains **transient runtime data** (active spell effects). Consider whether to migrate this table at all:
> - Buffs expire and are highly time-sensitive
> - Migration between servers means buffs may not function correctly
> - Standard practice: Let characters log in without buffs and rebuff naturally
> - **Recommendation**: Skip this table unless specifically needed

> **🔄 Field Mappings**
> 
> If migrating buffs, these fields require special handling:
> - `id` (TAKP) → `character_id` (PEQ) - Field rename
> - `bard_modifier` (TAKP) → `instrument_mod` (PEQ) - Field rename, type compatible
> - Set PEQ-exclusive fields to 0: `numhits`, `dot_rune`, `caston_x/y/z`
> - `bufftype` (TAKP) will be lost - purpose unknown

> **ℹ️ Buff Mechanics**
> 
> Understanding the shared fields:
> - `slot_id`: Which buff slot (1-N) the effect occupies
> - `ticsremaining`: Duration remaining in ticks (6 seconds per tick)
> - `counters`: Cure counter for disease/poison spells
> - `melee_rune/magic_rune`: Damage absorption amounts
> - `persistent`: Whether buff persists through zoning/logout

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
```sql
+--------------+-----------------------+------+-----+---------+-------+
| Field        | Type                  | Null | Key | Default | Extra |
+--------------+-----------------------+------+-----+---------+-------+
| character_id | int(10) unsigned      | NO   | PRI |         |       |
| slot_id      | tinyint(3) unsigned   | NO   | PRI |         |       |
| spell_id     | smallint(10) unsigned | NO   |     |         |       |
| caster_level | tinyint(3) unsigned   | NO   |     |         |       |
| caster_name  | varchar(64)           | NO   |     |         |       |
| ticsremaining| int(11)               | NO   |     |         |       |
| counters     | int(10) unsigned      | NO   |     |         |       |
| numhits      | int(10) unsigned      | NO   |     |         |       |
| melee_rune   | int(10) unsigned      | NO   |     |         |       |
| magic_rune   | int(10) unsigned      | NO   |     |         |       |
| persistent   | tinyint(3) unsigned   | NO   |     |         |       |
| dot_rune     | int(10)               | NO   |     | 0       |       |
| caston_x     | int(10)               | NO   |     | 0       |       |
| caston_y     | int(10)               | NO   |     | 0       |       |
| caston_z     | int(10)               | NO   |     | 0       |       |
| ExtraDIChance| int(10)               | NO   |     | 0       |       |
| instrument_mod| int(10)              | NO   |     | 10      |       |
+--------------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(character_id, slot_id)`

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
```sql
+---------------+-----------------------+------+-----+---------+-------+
| Field         | Type                  | Null | Key | Default | Extra |
+---------------+-----------------------+------+-----+---------+-------+
| id            | int(10)               | NO   | PRI | 0       |       |
| slot_id       | tinyint(3) unsigned   | NO   | PRI |         |       |
| spell_id      | smallint(10) unsigned | NO   |     |         |       |
| caster_level  | tinyint(3) unsigned   | NO   |     |         |       |
| caster_name   | varchar(64)           | NO   |     |         |       |
| ticsremaining | int(10) unsigned      | NO   |     |         |       |
| counters      | int(10) unsigned      | NO   |     |         |       |
| melee_rune    | int(10) unsigned      | NO   |     |         |       |
| magic_rune    | int(10) unsigned      | NO   |     |         |       |
| persistent    | tinyint(3) unsigned   | NO   |     |         |       |
| ExtraDIChance | int(10)               | NO   |     | 0       |       |
| bard_modifier | tinyint(3) unsigned   | NO   |     | 10      |       |
| bufftype      | int(11)               | NO   |     |         |       |
+---------------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(id, slot_id)`

</details>

### SQL Query Example

**⚠️ Not recommended to migrate buffs** - they are transient data. If you must migrate:
```sql
SELECT 
    id,                  -- maps to character_id
    slot_id,
    spell_id,
    caster_level,
    caster_name,
    ticsremaining,
    counters,
    0,                   -- numhits
    melee_rune,
    magic_rune,
    persistent,
    0,                   -- dot_rune
    0,                   -- caston_x
    0,                   -- caston_y
    0,                   -- caston_z
    ExtraDIChance,
    bard_modifier        -- maps to instrument_mod
FROM character_buffs
WHERE id = 12345;       -- Replace with actual character_id
```
