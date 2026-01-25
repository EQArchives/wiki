---
title: character_buffs
description: 
published: true
date: 2026-01-25T15:23:48.952Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T15:23:48.952Z
---

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