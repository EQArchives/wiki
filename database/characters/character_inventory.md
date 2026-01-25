---
title: character_inventory
description: 
published: true
date: 2026-01-25T16:38:27.358Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T16:16:52.564Z
---

## character_inventory / inventory

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (10 fields)</h4>
    <ul>
      <li><code>color</code></li>
      <li><code>augment_one</code> through <code>augment_six</code> (6 fields)</li>
      <li><code>instnodrop</code></li>
      <li><code>ornament_icon</code>, <code>ornament_idfile</code>, <code>ornament_hero_model</code></li>
      <li><code>guid</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (2 fields)</h4>
    <ul>
      <li><code>serialnumber</code></li>
      <li><code>initialserial</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>4 identical fields (with renames)</li>
      <li>1 shared field: <code>custom_data</code></li>
      <li>Total: 5 mappable fields</li>
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
    <td colspan="6" style="background-color: #f5f5f5; font-weight: bold; text-align: center;">
      📜 Classic EQ (1999-2001) (Compatible with TAKP)
    </td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>id</code> → <code>character_id</code></td>
    <td>int(11) unsigned PRI</td>
    <td>0</td>
    <td>int(11) PRI</td>
    <td>0</td>
    <td>Field renamed: <code>id</code> (TAKP) → <code>character_id</code> (PEQ)</td>
  </tr>
  
  <tr class="comparison-diff">
  	<td><code>slotid</code> → <code>slot_id</code></td>
    <td>mediumint(7) unsigned PRI</td>
    <td>0</td>
    <td>mediumint(7) unsigned PRI</td>
    <td>0</td>
    <td>Field renamed: <code>slotid</code> (TAKP) → <code>slot_id</code> (PEQ)</td>
  </tr>
  
  <tr class="comparison-diff">
  	<td><code>itemid</code> → <code>item_id</code></td>
    <td>int(11) unsigned NULL</td>
    <td>0</td>
    <td>int(11) unsigned NULL</td>
    <td>0</td>
    <td>Field renamed: <code>itemid</code> (TAKP) → <code>item_id</code> (PEQ)</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>charges</code></td>
    <td>smallint(3) unsigned NULL</td>
    <td>0</td>
    <td>smallint(3) unsigned NULL</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>custom_data</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>Identical - custom item data storage</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="6" style="background-color: #e3f2fd; font-weight: bold; text-align: center;">
      🔷 LDoN Expansion (Sept 2003) - PEQ Only
    </td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>augment_one</code></td>
    <td>mediumint(7) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (augment slot 1)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>augment_two</code> through <code>augment_six</code></td>
    <td colspan="5"><em>5 more augment slots - all set to 0</em></td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="6" style="background-color: #e3f2fd; font-weight: bold; text-align: center;">
      🔷 Gates of Discord (Feb 2004) - PEQ Only
    </td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>instnodrop</code></td>
    <td>tinyint(1) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (instance-specific no-drop flag)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="6" style="background-color: #e3f2fd; font-weight: bold; text-align: center;">
      🔷 Modern Expansions (Post-2010) - PEQ Only
    </td>
  </tr>
  
 <tr class="comparison-exclusive">
    <td><code>color</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (item tint/dye system)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ornament_icon</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (ornament icon - House of Thule+)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ornament_idfile</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (ornament graphics file)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ornament_hero_model</code></td>
    <td>int(11)</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (Hero's Forge model)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>guid</code></td>
    <td>bigint(20) unsigned NULL</td>
    <td>0</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (globally unique item identifier)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="6" style="background-color: #fff3e0; font-weight: bold; text-align: center;">
      ⚠️ TAKP-Specific Features (Lost on Migration)
    </td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>serialnumber</code></td>
    <td>—</td>
    <td>—</td>
    <td>int(10)</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Item serial tracking</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>initialserial</code></td>
    <td>—</td>
    <td>—</td>
    <td>tinyint(3)</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Initial serial flag</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **⚠️ Table Name Change**
> 
> Note the table name difference:
> - **TAKP**: `character_inventory`
> - **PEQ**: `inventory`

> **✅ Core Inventory Compatible**
> 
> The essential inventory data migrates successfully:
> - Item IDs, slot positions, and charges transfer directly
> - All equipped items, bags, and bank contents preserve
> - Custom data field maintains any special item properties

> **🔄 Field Mappings**
> 
> Simple field renames required:
> - `id` (TAKP) → `character_id` (PEQ)
> - `slotid` (TAKP) → `slot_id` (PEQ)
> - `itemid` (TAKP) → `item_id` (PEQ)
> - Set all augment slots (1-6) to 0 (augments are from LDoN+ expansions)
> - Set ornament fields to 0 (Hero's Forge from much later expansions)
> - Set `color` to 0, `instnodrop` to 0, `guid` to 0

> **⚠️ Data Loss**
> 
> TAKP-specific fields that will be lost:
> - `serialnumber` - Item serial number tracking
> - `initialserial` - Initial serial flag
> 
> These fields are TAKP-specific item tracking and don't have PEQ equivalents.

> **ℹ️ Inventory Slots**
> 
> Understanding `slot_id`:
> - 0-21: Equipped slots (charm, ears, head, etc.)
> - 22-29: General inventory slots
> - 2000-2007: Bank slots
> - 2500-2503: Shared bank slots
> - Bag contents use slot math (e.g., slot 22 bag slot 0 = 251)

> **📦 Expansion Features (PEQ Only)**
> 
> Set these to 0 - they're from expansions not in TAKP:
> - **Augments** (LDoN+): 6 augment slots per item
> - **Ornaments/Hero's Forge** (House of Thule+): Visual item overrides
> - **Item tinting** (`color`): Dye system
> - **GUID**: Globally unique item identifiers for trades/logs

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+---------------------+-----------------------+------+-----+---------+-------+
| Field               | Type                  | Null | Key | Default | Extra |
+---------------------+-----------------------+------+-----+---------+-------+
| character_id        | int(11) unsigned      | NO   | PRI | 0       |       |
| slot_id             | mediumint(7) unsigned | NO   | PRI | 0       |       |
| item_id             | int(11) unsigned      | YES  |     | 0       |       |
| charges             | smallint(3) unsigned  | YES  |     | 0       |       |
| color               | int(11) unsigned      | NO   |     | 0       |       |
| augment_one         | mediumint(7) unsigned | NO   |     | 0       |       |
| augment_two         | mediumint(7) unsigned | NO   |     | 0       |       |
| augment_three       | mediumint(7) unsigned | NO   |     | 0       |       |
| augment_four        | mediumint(7) unsigned | NO   |     | 0       |       |
| augment_five        | mediumint(7) unsigned | NO   |     | 0       |       |
| augment_six         | mediumint(7) unsigned | NO   |     | 0       |       |
| instnodrop          | tinyint(1) unsigned   | NO   |     | 0       |       |
| custom_data         | text                  | YES  |     | NULL    |       |
| ornament_icon       | int(11) unsigned      | NO   |     | 0       |       |
| ornament_idfile     | int(11) unsigned      | NO   |     | 0       |       |
| ornament_hero_model | int(11)               | NO   |     | 0       |       |
| guid                | bigint(20) unsigned   | YES  |     | 0       |       |
+---------------------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(character_id, slot_id)`  
**Table Name**: `inventory`

</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+---------------+-----------------------+------+-----+---------+-------+
| Field         | Type                  | Null | Key | Default | Extra |
+---------------+-----------------------+------+-----+---------+-------+
| id            | int(11)               | NO   | PRI | 0       |       |
| slotid        | mediumint(7) unsigned | NO   | PRI | 0       |       |
| itemid        | int(11) unsigned      | YES  |     | 0       |       |
| charges       | smallint(3) unsigned  | YES  |     | 0       |       |
| custom_data   | text                  | YES  |     | NULL    |       |
| serialnumber  | int(10)               | NO   |     | 0       |       |
| initialserial | tinyint(3)            | NO   |     | 0       |       |
+---------------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(id, slotid)`  
**Table Name**: `character_inventory`

</details>

### SQL Query Example

The following query in a TAKP schema will retrieve character inventory data for migration to PEQ.
```sql
SELECT 
    id AS character_id,
    slotid AS slot_id,
    itemid AS item_id,
    charges,
    0 AS color,              -- PEQ only
    0 AS augment_one,        -- PEQ only
    0 AS augment_two,        -- PEQ only
    0 AS augment_three,      -- PEQ only
    0 AS augment_four,       -- PEQ only
    0 AS augment_five,       -- PEQ only
    0 AS augment_six,        -- PEQ only
    0 AS instnodrop,         -- PEQ only
    custom_data,
    0 AS ornament_icon,      -- PEQ only
    0 AS ornament_idfile,    -- PEQ only
    0 AS ornament_hero_model,-- PEQ only
    0 AS guid                -- PEQ only
FROM character_inventory
WHERE id = 12345;            -- Replace with actual character_id
```