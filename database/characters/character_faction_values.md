---
title: character_faction_values
description: 
published: true
date: 2026-01-25T02:15:16.451Z
tags: database, takp_peq_migration, character_faction_values
editor: markdown
dateCreated: 2026-01-25T02:15:16.451Z
---

## character_faction_values

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (0 fields)</h4>
    <ul>
      <li><em>None - all PEQ fields map to TAKP</em></li>
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
      <li>3 identical fields</li>
      <li>1 field with rename</li>
      <li>Total: 4 fully compatible fields</li>
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
    <td><code>char_id</code> / <code>id</code></td>
    <td>int(4) PRI</td>
    <td>0</td>
    <td>int(11) PRI</td>
    <td>0</td>
    <td>Field renamed: <code>id</code> (TAKP) → <code>char_id</code> (PEQ). Type size differs but compatible</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>faction_id</code></td>
    <td>int(4) PRI</td>
    <td>0</td>
    <td>int(4) PRI</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>current_value</code></td>
    <td>smallint(6)</td>
    <td>0</td>
    <td>smallint(6)</td>
    <td>0</td>
    <td>Identical - faction standing value</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>temp</code></td>
    <td>tinyint(3)</td>
    <td>0</td>
    <td>tinyint(3)</td>
    <td>0</td>
    <td>Identical - temporary faction modifier</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **✅ Excellent Compatibility**
> 
> This table has nearly perfect compatibility between TAKP and PEQ:
> - Only difference is field name: `id` (TAKP) → `char_id` (PEQ)
> - All faction data transfers directly without conversion
> - Primary key structure identical: `(char_id/id, faction_id)`

> **🔄 Field Mappings**
> 
> Simple field rename required:
> - `id` (TAKP) → `char_id` (PEQ) - Same data, just renamed for consistency
> - Type differs (`int(11)` vs `int(4)`) but compatible - both store character IDs

> **ℹ️ Faction System**
> 
> Understanding the fields:
> - `char_id`/`id`: Character identifier
> - `faction_id`: Which faction (e.g., Guards of Qeynos, Clan Crushbone)
> - `current_value`: Current standing (-2000 to +2000, where -750+ is ally, -500 to -749 is warmly, etc.)
> - `temp`: Temporary faction modifier (usually from quest or spell effects)

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| char_id       | int(4)      | NO   | PRI | 0       |       |
| faction_id    | int(4)      | NO   | PRI | 0       |       |
| current_value | smallint(6) | NO   |     | 0       |       |
| temp          | tinyint(3)  | NO   |     | 0       |       |
+---------------+-------------+------+-----+---------+-------+
```

**Primary Key**: `(char_id, faction_id)`

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| id            | int(11)     | NO   | PRI | 0       |       |
| faction_id    | int(4)      | NO   | PRI | 0       |       |
| current_value | smallint(6) | NO   |     | 0       |       |
| temp          | tinyint(3)  | NO   |     | 0       |       |
+---------------+-------------+------+-----+---------+-------+
```

**Primary Key**: `(id, faction_id)`

</details>

### SQL Query Example

The following query in a TAKP schema will retrieve character faction data for migration to PEQ.
```sql
SELECT 
    id AS char_id,     -- Field rename
    faction_id,
    current_value,
    temp
FROM character_faction_values
WHERE id = 12345;      -- Replace with actual character_id
```