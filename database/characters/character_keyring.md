---
title: character_keyring
description: 
published: true
date: 2026-01-25T22:49:43.092Z
tags: database, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T22:27:28.217Z
---

## character_keyring / keyring

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (1 field)</h4>
    <ul>
      <li><code>id</code> (auto_increment primary key)</li>
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
      <li>2 fields with rename</li>
      <li><strong>⚠️ Table name differs</strong></li>
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
  <tr class="comparison-exclusive">
    <td><code>id</code></td>
    <td>int(10) unsigned PRI AI</td>
    <td>NULL</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Auto-increment primary key (not needed for migration)</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>id</code> → <code>char_id</code></td>
    <td>int(11) MUL</td>
    <td>0</td>
    <td>int(11)</td>
    <td>0</td>
    <td>Field renamed: <code>id</code> (TAKP) → <code>char_id</code> (PEQ)</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>item_id</code></td>
    <td>int(11)</td>
    <td>0</td>
    <td>int(11)</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **⚠️ Table Name Change**
> 
> Note the table name difference:
> - **TAKP**: `character_keyring`
> - **PEQ**: `keyring`

> **⚠️ Primary Key Difference**
> 
> The primary key structure differs:
> - **TAKP**: Composite key on `(id, item_id)` implied (no explicit PRI)
> - **PEQ**: Auto-increment `id` field as primary key, with `char_id` as indexed field
> 
> PEQ uses a surrogate key system, so you don't need to provide the `id` field - it will auto-generate.

> **🔄 Field Mappings**
> 
> Simple field rename required:
> - `id` (TAKP) → `char_id` (PEQ)
> - `item_id` → `item_id` (same)
> - Do NOT provide `id` field in INSERT - let PEQ auto-generate it

> **ℹ️ Keyring System**
> 
> The keyring stores keys (quest items) that persist across character logins:
> - Each row represents one key item the character owns
> - Keys are typically quest items that unlock doors or gates
> - Common keys: Soulfire key, Iksar skull key, etc.

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+---------+------------------+------+-----+---------+----------------+
| Field   | Type             | Null | Key | Default | Extra          |
+---------+------------------+------+-----+---------+----------------+
| id      | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| char_id | int(11)          | NO   | MUL | 0       |                |
| item_id | int(11)          | NO   |     | 0       |                |
+---------+------------------+------+-----+---------+----------------+
```

**Table Name**: `keyring`  
**Primary Key**: `id` (auto_increment)  
**Index**: `char_id` (MUL)

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+---------+---------+------+-----+---------+-------+
| Field   | Type    | Null | Key | Default | Extra |
+---------+---------+------+-----+---------+-------+
| id      | int(11) | NO   |     | 0       |       |
| item_id | int(11) | NO   |     | 0       |       |
+---------+---------+------+-----+---------+-------+
```

**Table Name**: `character_keyring`  
**Primary Key**: Likely composite `(id, item_id)`

</details>

### SQL Query Example

The following query migrates keyring data from TAKP to PEQ. Note the table name change and field rename.
```sql
-- Read from TAKP
SELECT 
    id AS char_id,  -- Field rename
    item_id
FROM character_keyring
WHERE id = 12345;   -- Replace with actual character_id

-- Insert into PEQ (do NOT provide 'id' field - it auto-generates)
INSERT INTO keyring (char_id, item_id)
VALUES (12345, <item_id>);
```