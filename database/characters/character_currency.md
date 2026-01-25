---
title: character_currency
description: 
published: true
date: 2026-01-25T02:30:34.716Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T02:30:34.716Z
---

## character_currency

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (4 fields)</h4>
    <ul>
      <li><code>radiant_crystals</code></li>
      <li><code>career_radiant_crystals</code></li>
      <li><code>ebon_crystals</code></li>
      <li><code>career_ebon_crystals</code></li>
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
      <li>13 identical fields</li>
      <li>0 fields with differences</li>
      <li>Total: 13 mappable fields</li>
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
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>platinum</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>gold</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>silver</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>copper</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>platinum_bank</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>gold_bank</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>silver_bank</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>copper_bank</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>platinum_cursor</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>gold_cursor</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>silver_cursor</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>copper_cursor</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>radiant_crystals</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (alternative currency from later expansions)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>career_radiant_crystals</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (lifetime total tracker)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ebon_crystals</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (alternative currency from later expansions)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>career_ebon_crystals</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 on migration (lifetime total tracker)</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **✅ High Compatibility**
> 
> This table has excellent compatibility between TAKP and PEQ. All TAKP currency data migrates directly with no conversions needed.

> **💰 Currency Structure**
> 
> Both schemas track three currency locations:
> - **Inventory**: platinum, gold, silver, copper (money on character)
> - **Bank**: platinum_bank, gold_bank, silver_bank, copper_bank (money in bank)
> - **Cursor**: platinum_cursor, gold_cursor, silver_cursor, copper_cursor (money being held/traded)

> **🔄 Field Mappings**
> 
> No special handling required:
> - All 13 TAKP fields map 1:1 to PEQ
> - Alternative currency fields (radiant/ebon crystals) are set to 0
> - These crystals are from post-classic expansions not present in TAKP

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+-------------------------+------------------+------+-----+---------+-------+
| Field                   | Type             | Null | Key | Default | Extra |
+-------------------------+------------------+------+-----+---------+-------+
| id                      | int(11) unsigned | NO   | PRI | 0       |       |
| platinum                | int(11) unsigned | NO   |     | 0       |       |
| gold                    | int(11) unsigned | NO   |     | 0       |       |
| silver                  | int(11) unsigned | NO   |     | 0       |       |
| copper                  | int(11) unsigned | NO   |     | 0       |       |
| platinum_bank           | int(11) unsigned | NO   |     | 0       |       |
| gold_bank               | int(11) unsigned | NO   |     | 0       |       |
| silver_bank             | int(11) unsigned | NO   |     | 0       |       |
| copper_bank             | int(11) unsigned | NO   |     | 0       |       |
| platinum_cursor         | int(11) unsigned | NO   |     | 0       |       |
| gold_cursor             | int(11) unsigned | NO   |     | 0       |       |
| silver_cursor           | int(11) unsigned | NO   |     | 0       |       |
| copper_cursor           | int(11) unsigned | NO   |     | 0       |       |
| radiant_crystals        | int(11) unsigned | NO   |     | 0       |       |
| career_radiant_crystals | int(11) unsigned | NO   |     | 0       |       |
| ebon_crystals           | int(11) unsigned | NO   |     | 0       |       |
| career_ebon_crystals    | int(11) unsigned | NO   |     | 0       |       |
+-------------------------+------------------+------+-----+---------+-------+
```

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+-----------------+------------------+------+-----+---------+-------+
| Field           | Type             | Null | Key | Default | Extra |
+-----------------+------------------+------+-----+---------+-------+
| id              | int(11) unsigned | NO   | PRI | 0       |       |
| platinum        | int(11) unsigned | NO   |     | 0       |       |
| gold            | int(11) unsigned | NO   |     | 0       |       |
| silver          | int(11) unsigned | NO   |     | 0       |       |
| copper          | int(11) unsigned | NO   |     | 0       |       |
| platinum_bank   | int(11) unsigned | NO   |     | 0       |       |
| gold_bank       | int(11) unsigned | NO   |     | 0       |       |
| silver_bank     | int(11) unsigned | NO   |     | 0       |       |
| copper_bank     | int(11) unsigned | NO   |     | 0       |       |
| platinum_cursor | int(11) unsigned | NO   |     | 0       |       |
| gold_cursor     | int(11) unsigned | NO   |     | 0       |       |
| silver_cursor   | int(11) unsigned | NO   |     | 0       |       |
| copper_cursor   | int(11) unsigned | NO   |     | 0       |       |
+-----------------+------------------+------+-----+---------+-------+
```

</details>

### SQL Query Example

The following query in a TAKP schema will retrieve character currency data for migration to PEQ.
```sql
SELECT 
    id,
    platinum,
    gold,
    silver,
    copper,
    platinum_bank,
    gold_bank,
    silver_bank,
    copper_bank,
    platinum_cursor,
    gold_cursor,
    silver_cursor,
    copper_cursor,
    0,  -- radiant_crystals
    0,  -- career_radiant_crystals
    0,  -- ebon_crystals
    0   -- career_ebon_crystals
FROM character_currency
WHERE id = ?;  -- character_id
```