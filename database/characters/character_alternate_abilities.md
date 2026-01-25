---
title: character_alternate_abilities
description: 
published: true
date: 2026-01-25T15:18:46.038Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T15:17:44.350Z
---

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