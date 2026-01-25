---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-25T02:35:11.066Z
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

## character_alternate_abilities

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (1 field)</h4>
    <ul>
      <li><code>charges</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (1 field)</h4>
    <ul>
      <li><code>slot</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>3 identical fields</li>
      <li>0 fields with differences</li>
      <li><strong>⚠️ Primary key structure differs</strong></li>
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
  <tr class="comparison-same">
    <td><code>id</code></td>
    <td>int(11) unsigned PRI</td>
    <td>0</td>
    <td>int(11) unsigned PRI</td>
    <td>0</td>
    <td>Identical - character ID</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>slot</code></td>
    <td>—</td>
    <td>—</td>
    <td>smallint(11) unsigned PRI</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Slot number for AA ordering. Lost on migration.</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>aa_id</code></td>
    <td>smallint(11) unsigned <strong>PRI</strong></td>
    <td>0</td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>Part of primary key in PEQ, not in TAKP</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>aa_value</code></td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>Identical - rank/level of the AA ability</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>charges</code></td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (for rechargeable AA abilities)</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **⚠️ Critical Incompatibility**
> 
> This table has **significant structural differences** that require careful migration:
> - **Primary key differs**: TAKP uses `(id, slot)`, PEQ uses `(id, aa_id)`
> - **Slot field lost**: TAKP's slot ordering system is not preserved in PEQ
> - **Duplicate AA risk**: If a character has the same AA at different slots in TAKP, migration will fail due to PEQ's unique constraint on `(id, aa_id)`

> **🔄 Field Mappings**
> 
> Migration strategy:
> - Drop `slot` field (TAKP only - not needed in PEQ)
> - Add `charges` field set to 0 (PEQ only - for clickable AAs)
> - **Important**: Group by `(id, aa_id)` and take MAX(aa_value) to handle potential duplicates
> - PEQ uses `aa_id` as part of primary key instead of `slot`

> **⚠️ Data Integrity Notes**
> 
> - If TAKP allows duplicate `aa_id` values per character (different slots), migration requires deduplication
> - Recommend validating no duplicate `(id, aa_id)` combinations exist before migration
> - The `slot` field loss means AA ordering/hotbar positions are not preserved

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+----------+-----------------------+------+-----+---------+-------+
| Field    | Type                  | Null | Key | Default | Extra |
+----------+-----------------------+------+-----+---------+-------+
| id       | int(11) unsigned      | NO   | PRI | 0       |       |
| aa_id    | smallint(11) unsigned | NO   | PRI | 0       |       |
| aa_value | smallint(11) unsigned | NO   |     | 0       |       |
| charges  | smallint(11) unsigned | NO   |     | 0       |       |
+----------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(id, aa_id)`

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+----------+-----------------------+------+-----+---------+-------+
| Field    | Type                  | Null | Key | Default | Extra |
+----------+-----------------------+------+-----+---------+-------+
| id       | int(11) unsigned      | NO   | PRI | 0       |       |
| slot     | smallint(11) unsigned | NO   | PRI | 0       |       |
| aa_id    | smallint(11) unsigned | NO   |     | 0       |       |
| aa_value | smallint(11) unsigned | NO   |     | 0       |       |
+----------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(id, slot)`

</details>

### SQL Query Example

The following query in a TAKP schema will retrieve character AA data for migration to PEQ, handling potential duplicates.
```sql
-- Check for duplicate aa_id per character (should return 0 rows)
SELECT id, aa_id, COUNT(*) as count
FROM character_alternate_abilities
WHERE id = ?  -- character_id
GROUP BY id, aa_id
HAVING COUNT(*) > 1;

-- Migration query (deduplicates by taking highest aa_value if duplicates exist)
SELECT 
    id,
    aa_id,
    MAX(aa_value) as aa_value,  -- Take highest rank if duplicates
    0 as charges                 -- Always 0 for classic AAs
FROM character_alternate_abilities
WHERE id = ?  -- character_id
GROUP BY id, aa_id;
```

> **💡 Migration Tip**: Run the duplicate check query first. If it returns any rows, investigate why the same AA exists in multiple slots before proceeding with migration.

## character_bind

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (1 field)</h4>
    <ul>
      <li><code>instance_id</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (0 fields)</h4>
    <ul>
      <li><em>None - all TAKP fields map to PEQ</em></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>5 identical fields</li>
      <li>1 field with rename</li>
      <li>Total: 6 mappable fields</li>
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
  <tr class="comparison-same">
    <td><code>id</code></td>
    <td>int(11) unsigned PRI AI</td>
    <td>NULL</td>
    <td>int(11) unsigned PRI AI</td>
    <td>NULL</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>slot</code> / <code>is_home</code></td>
    <td>int(4) PRI</td>
    <td>0</td>
    <td>tinyint(11) unsigned PRI</td>
    <td>0</td>
    <td>Renamed field: <code>is_home</code> (TAKP) → <code>slot</code> (PEQ). Type differs but compatible</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>zone_id</code></td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>smallint(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>instance_id</code></td>
    <td>mediumint(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (no instances in TAKP)</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>x</code></td>
    <td>float</td>
    <td>0</td>
    <td>float</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>y</code></td>
    <td>float</td>
    <td>0</td>
    <td>float</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>z</code></td>
    <td>float</td>
    <td>0</td>
    <td>float</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>heading</code></td>
    <td>float</td>
    <td>0</td>
    <td>float</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **ℹ️ Bind Point Compatibility**
> 
> The `character_bind` table stores character bind locations (home city bind and secondary bind).
> - TAKP uses `is_home` (0 = secondary bind, 1 = home bind)
> - PEQ uses `slot` (0 = primary bind, 1 = secondary bind)
> - Both systems support 2 bind points maximum

> **🔄 Field Mappings**
> 
> The following fields require special handling:
> - `is_home` (TAKP) → `slot` (PEQ) - Direct mapping, values are compatible
> - Set `instance_id` to 0 (PEQ doesn't use instances for bind points in TAKP era)

> **⚠️ Important Notes**
> 
> - Only bind slots 0 and 1 should be migrated from TAKP
> - PEQ may support additional bind slots in newer content
> - Coordinate values (x, y, z, heading) transfer directly without modification

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+-------------+------------------------+------+-----+---------+----------------+
| Field       | Type                   | Null | Key | Default | Extra          |
+-------------+------------------------+------+-----+---------+----------------+
| id          | int(11) unsigned       | NO   | PRI | NULL    | auto_increment |
| slot        | int(4)                 | NO   | PRI | 0       |                |
| zone_id     | smallint(11) unsigned  | NO   |     | 0       |                |
| instance_id | mediumint(11) unsigned | NO   |     | 0       |                |
| x           | float                  | NO   |     | 0       |                |
| y           | float                  | NO   |     | 0       |                |
| z           | float                  | NO   |     | 0       |                |
| heading     | float                  | NO   |     | 0       |                |
+-------------+------------------------+------+-----+---------+----------------+
```

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+---------+-----------------------+------+-----+---------+----------------+
| Field   | Type                  | Null | Key | Default | Extra          |
+---------+-----------------------+------+-----+---------+----------------+
| id      | int(11) unsigned      | NO   | PRI | NULL    | auto_increment |
| is_home | tinyint(11) unsigned  | NO   | PRI | 0       |                |
| zone_id | smallint(11) unsigned | NO   |     | 0       |                |
| x       | float                 | NO   |     | 0       |                |
| y       | float                 | NO   |     | 0       |                |
| z       | float                 | NO   |     | 0       |                |
| heading | float                 | NO   |     | 0       |                |
+---------+-----------------------+------+-----+---------+----------------+
```

</details>

### SQL Query Example

The following query in a TAKP schema will retrieve character bind data for migration to PEQ.
```sql
SELECT 
    id,
    is_home,              -- maps to slot
    zone_id,
    0,                    -- instance_id (always 0)
    x,
    y,
    z,
    heading
FROM character_bind
WHERE id = ?            -- character_id
  AND is_home IN (0, 1); -- only migrate primary/secondary binds
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

## character_data
#### PEQ Format
```sql
+-------------------------+-----------------------+------+-----+---------+----------------+
| Field                   | Type                  | Null | Key | Default | Extra          |
+-------------------------+-----------------------+------+-----+---------+----------------+
| id                      | int(11) unsigned      | NO   | PRI | NULL    | auto_increment |
| account_id              | int(11)               | NO   | MUL | 0       |                |
| name                    | varchar(64)           | NO   | UNI |         |                |
| last_name               | varchar(64)           | NO   |     |         |                |
| title                   | varchar(32)           | NO   |     |         |                |
| suffix                  | varchar(32)           | NO   |     |         |                |
| zone_id                 | int(11) unsigned      | NO   |     | 0       |                |
| zone_instance           | int(11) unsigned      | NO   |     | 0       |                |
| y                       | float                 | NO   |     | 0       |                |
| x                       | float                 | NO   |     | 0       |                |
| z                       | float                 | NO   |     | 0       |                |
| heading                 | float                 | NO   |     | 0       |                |
| gender                  | tinyint(11) unsigned  | NO   |     | 0       |                |
| race                    | smallint(11) unsigned | NO   |     | 0       |                |
| class                   | tinyint(11) unsigned  | NO   |     | 0       |                |
| level                   | int(11) unsigned      | NO   |     | 0       |                |
| deity                   | int(11) unsigned      | NO   |     | 0       |                |
| birthday                | int(11) unsigned      | NO   |     | 0       |                |
| last_login              | int(11) unsigned      | NO   |     | 0       |                |
| time_played             | int(11) unsigned      | NO   |     | 0       |                |
| level2                  | tinyint(11) unsigned  | NO   |     | 0       |                |
| anon                    | tinyint(11) unsigned  | NO   |     | 0       |                |
| gm                      | tinyint(11) unsigned  | NO   |     | 0       |                |
| face                    | int(11) unsigned      | NO   |     | 0       |                |
| hair_color              | tinyint(11) unsigned  | NO   |     | 0       |                |
| hair_style              | tinyint(11) unsigned  | NO   |     | 0       |                |
| beard                   | tinyint(11) unsigned  | NO   |     | 0       |                |
| beard_color             | tinyint(11) unsigned  | NO   |     | 0       |                |
| eye_color_1             | tinyint(11) unsigned  | NO   |     | 0       |                |
| eye_color_2             | tinyint(11) unsigned  | NO   |     | 0       |                |
| drakkin_heritage        | int(11) unsigned      | NO   |     | 0       |                |
| drakkin_tattoo          | int(11) unsigned      | NO   |     | 0       |                |
| drakkin_details         | int(11) unsigned      | NO   |     | 0       |                |
| ability_time_seconds    | tinyint(11) unsigned  | NO   |     | 0       |                |
| ability_number          | tinyint(11) unsigned  | NO   |     | 0       |                |
| ability_time_minutes    | tinyint(11) unsigned  | NO   |     | 0       |                |
| ability_time_hours      | tinyint(11) unsigned  | NO   |     | 0       |                |
| exp                     | int(11) unsigned      | NO   |     | 0       |                |
| exp_enabled             | tinyint(1) unsigned   | NO   |     | 1       |                |
| aa_points_spent         | int(11) unsigned      | NO   |     | 0       |                |
| aa_exp                  | int(11) unsigned      | NO   |     | 0       |                |
| aa_points               | int(11) unsigned      | NO   |     | 0       |                |
| group_leadership_exp    | int(11) unsigned      | NO   |     | 0       |                |
| raid_leadership_exp     | int(11) unsigned      | NO   |     | 0       |                |
| group_leadership_points | int(11) unsigned      | NO   |     | 0       |                |
| raid_leadership_points  | int(11) unsigned      | NO   |     | 0       |                |
| points                  | int(11) unsigned      | NO   |     | 0       |                |
| cur_hp                  | int(11) unsigned      | NO   |     | 0       |                |
| mana                    | int(11) unsigned      | NO   |     | 0       |                |
| endurance               | int(11) unsigned      | NO   |     | 0       |                |
| intoxication            | int(11) unsigned      | NO   |     | 0       |                |
| str                     | int(11) unsigned      | NO   |     | 0       |                |
| sta                     | int(11) unsigned      | NO   |     | 0       |                |
| cha                     | int(11) unsigned      | NO   |     | 0       |                |
| dex                     | int(11) unsigned      | NO   |     | 0       |                |
| int                     | int(11) unsigned      | NO   |     | 0       |                |
| agi                     | int(11) unsigned      | NO   |     | 0       |                |
| wis                     | int(11) unsigned      | NO   |     | 0       |                |
| extra_haste             | int(11)               | NO   |     | 0       |                |
| zone_change_count       | int(11) unsigned      | NO   |     | 0       |                |
| toxicity                | int(11) unsigned      | NO   |     | 0       |                |
| hunger_level            | int(11) unsigned      | NO   |     | 0       |                |
| thirst_level            | int(11) unsigned      | NO   |     | 0       |                |
| ability_up              | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_guk         | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_mir         | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_mmc         | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_ruj         | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_tak         | int(11) unsigned      | NO   |     | 0       |                |
| ldon_points_available   | int(11) unsigned      | NO   |     | 0       |                |
| tribute_time_remaining  | int(11) unsigned      | NO   |     | 0       |                |
| career_tribute_points   | int(11) unsigned      | NO   |     | 0       |                |
| tribute_points          | int(11) unsigned      | NO   |     | 0       |                |
| tribute_active          | int(11) unsigned      | NO   |     | 0       |                |
| pvp_status              | tinyint(11) unsigned  | NO   |     | 0       |                |
| pvp_kills               | int(11) unsigned      | NO   |     | 0       |                |
| pvp_deaths              | int(11) unsigned      | NO   |     | 0       |                |
| pvp_current_points      | int(11) unsigned      | NO   |     | 0       |                |
| pvp_career_points       | int(11) unsigned      | NO   |     | 0       |                |
| pvp_best_kill_streak    | int(11) unsigned      | NO   |     | 0       |                |
| pvp_worst_death_streak  | int(11) unsigned      | NO   |     | 0       |                |
| pvp_current_kill_streak | int(11) unsigned      | NO   |     | 0       |                |
| pvp2                    | int(11) unsigned      | NO   |     | 0       |                |
| pvp_type                | int(11) unsigned      | NO   |     | 0       |                |
| show_helm               | int(11) unsigned      | NO   |     | 0       |                |
| group_auto_consent      | tinyint(11) unsigned  | NO   |     | 0       |                |
| raid_auto_consent       | tinyint(11) unsigned  | NO   |     | 0       |                |
| guild_auto_consent      | tinyint(11) unsigned  | NO   |     | 0       |                |
| leadership_exp_on       | tinyint(11) unsigned  | NO   |     | 0       |                |
| RestTimer               | int(11) unsigned      | NO   |     | 0       |                |
| air_remaining           | int(11) unsigned      | NO   |     | 0       |                |
| autosplit_enabled       | int(11) unsigned      | NO   |     | 0       |                |
| lfp                     | tinyint(1) unsigned   | NO   |     | 0       |                |
| lfg                     | tinyint(1) unsigned   | NO   |     | 0       |                |
| mailkey                 | char(16)              | NO   |     |         |                |
| xtargets                | tinyint(3) unsigned   | NO   |     | 5       |                |
| first_login             | int(11) unsigned      | NO   |     | 0       |                |
| ingame                  | tinyint(1) unsigned   | NO   |     | 0       |                |
| e_aa_effects            | int(11) unsigned      | NO   |     | 0       |                |
| e_percent_to_aa         | int(11) unsigned      | NO   |     | 0       |                |
| e_expended_aa_spent     | int(11) unsigned      | NO   |     | 0       |                |
| aa_points_spent_old     | int(11) unsigned      | NO   |     | 0       |                |
| aa_points_old           | int(11) unsigned      | NO   |     | 0       |                |
| e_last_invsnapshot      | int(11) unsigned      | NO   |     | 0       |                |
| deleted_at              | datetime              | YES  |     | NULL    |                |
| illusion_block          | tinyint(11) unsigned  | NO   |     | 0       |                |
+-------------------------+-----------------------+------+-----+---------+----------------+
106 rows in set (0.002 sec)
```
