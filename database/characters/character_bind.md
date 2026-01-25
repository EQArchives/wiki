---
title: character_bind
description: 
published: true
date: 2026-01-25T15:21:19.858Z
tags: database, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T15:20:46.369Z
---

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
