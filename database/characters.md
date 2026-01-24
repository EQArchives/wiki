---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-24T23:50:37.163Z
tags: database
editor: markdown
dateCreated: 2026-01-20T03:13:49.975Z
---

# Character Migration
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
## account 

## Tabs {.tabset}

### Account Table Schema Comparison
<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (4 fields)</h4>
    <ul>
      <li><code>auto_login_charname</code></li>
      <li><code>ls_id</code></li>
      <li><code>crc_eqgame</code></li>
      <li><code>crc_skillcaps</code></li>
      <li><code>crc_basedata</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (5 fields)</h4>
    <ul>
      <li><code>forum_id</code></li>
      <li><code>expansion</code></li>
      <li><code>active</code></li>
      <li><code>ip_exemption_multiplier</code></li>
      <li><code>mule</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>13 identical fields</li>
      <li>7 fields with minor differences</li>
      <li>Total: 20 mappable fields</li>
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
    <td>int(11) PRI AI</td>
    <td>NULL</td>
    <td>int(11) PRI AI</td>
    <td>NULL</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>name</code></td>
    <td>varchar(30) <strong>MUL</strong></td>
    <td>''</td>
    <td>varchar(30) <strong>UNI</strong></td>
    <td>NULL</td>
    <td>Index type differs: non-unique vs unique</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>charname</code></td>
    <td>varchar(64)</td>
    <td>''</td>
    <td>varchar(64)</td>
    <td>NULL</td>
    <td>Type compatible</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>auto_login_charname</code></td>
    <td>varchar(64)</td>
    <td>''</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to empty string on migration</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>sharedplat</code></td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>int(11) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>password</code></td>
    <td>varchar(50)</td>
    <td>''</td>
    <td>varchar(50)</td>
    <td>NULL</td>
    <td>Type compatible</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>status</code></td>
    <td>int(5)</td>
    <td>0</td>
    <td>int(5)</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ls_id</code></td>
    <td>varchar(64) MUL</td>
    <td>eqemu</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 'local' on migration</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>lsaccount_id</code></td>
    <td>int(11) unsigned NULL</td>
    <td>NULL</td>
    <td>int(11) unsigned <strong>UNI</strong> NULL</td>
    <td>NULL</td>
    <td>TAKP adds unique index</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>forum_id</code></td>
    <td>—</td>
    <td>—</td>
    <td>int(10)</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>gmspeed</code></td>
    <td>tinyint(3) unsigned</td>
    <td>0</td>
    <td>tinyint(3) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>invulnerable</code> / <code>gminvul</code></td>
    <td>tinyint(4) <strong>NULL</strong></td>
    <td>0</td>
    <td>tinyint(4) <strong>NOT NULL</strong></td>
    <td>0</td>
    <td>Renamed field, PEQ allows NULL</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>flymode</code></td>
    <td>tinyint(4) <strong>NULL</strong></td>
    <td>0</td>
    <td>tinyint(4) <strong>NOT NULL</strong></td>
    <td>0</td>
    <td>PEQ allows NULL, TAKP doesn't</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>ignore_tells</code></td>
    <td>tinyint(4) <strong>NULL</strong></td>
    <td>0</td>
    <td>tinyint(4) <strong>NOT NULL</strong></td>
    <td>0</td>
    <td>PEQ allows NULL, TAKP doesn't</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>revoked</code></td>
    <td>tinyint(3) unsigned</td>
    <td>0</td>
    <td>tinyint(3) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>karma</code></td>
    <td>int(5) unsigned</td>
    <td>0</td>
    <td>int(5) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>minilogin_ip</code></td>
    <td>varchar(32)</td>
    <td>''</td>
    <td>varchar(32)</td>
    <td>NULL</td>
    <td>Type compatible</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>hideme</code></td>
    <td>tinyint(4)</td>
    <td>0</td>
    <td>tinyint(4)</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>rulesflag</code></td>
    <td>tinyint(1) unsigned</td>
    <td>0</td>
    <td>tinyint(1) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-diff">
    <td><code>suspendeduntil</code></td>
    <td>datetime <strong>NULL</strong></td>
    <td>NULL</td>
    <td>datetime <strong>NOT NULL</strong></td>
    <td>0000-00-00 00:00:00</td>
    <td>Nullable and default value differ</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>time_creation</code></td>
    <td>int(10) unsigned</td>
    <td>0</td>
    <td>int(10) unsigned</td>
    <td>0</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>expansion</code></td>
    <td>—</td>
    <td>—</td>
    <td>tinyint(4)</td>
    <td>12</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>ban_reason</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td><code>suspend_reason</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>Identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>crc_eqgame</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Client validation field</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>crc_skillcaps</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Client validation field</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>crc_basedata</code></td>
    <td>text NULL</td>
    <td>NULL</td>
    <td>—</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Client validation field</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>active</code></td>
    <td>—</td>
    <td>—</td>
    <td>tinyint(4)</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>ip_exemption_multiplier</code></td>
    <td>—</td>
    <td>—</td>
    <td>int(5) NULL</td>
    <td>1</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td><code>mule</code></td>
    <td>—</td>
    <td>—</td>
    <td>tinyint(4)</td>
    <td>0</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **⚠️ Data Loss Warning**
> 
> When migrating from TAKP to PEQ, the following fields will be lost:
> - `forum_id` - Forum integration data
> - `expansion` - Expansion flag settings
> - `active` - Account status flags
> - `ip_exemption_multiplier` - Connection limit settings
> - `mule` - Mule account flags

> **🔄 Field Mappings**
> 
> The following fields require special handling:
> - `gminvul` (TAKP) → `invulnerable` (PEQ)
> - Set `ls_id` to 'local' (PEQ default)
> - Set `auto_login_charname` to empty string
> - Handle NULL constraints for `flymode`, `ignore_tells`, `suspendeduntil`

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema</strong></summary>
  
```sql
+---------------------+---------------------+------+-----+---------+----------------+
| Field               | Type                | Null | Key | Default | Extra          |
+---------------------+---------------------+------+-----+---------+----------------+
| id                  | int(11)             | NO   | PRI | NULL    | auto_increment |
| name                | varchar(30)         | NO   | MUL |         |                |
| charname            | varchar(64)         | NO   |     |         |                |
| auto_login_charname | varchar(64)         | NO   |     |         |                |
| sharedplat          | int(11) unsigned    | NO   |     | 0       |                |
| password            | varchar(50)         | NO   |     |         |                |
| status              | int(5)              | NO   |     | 0       |                |
| ls_id               | varchar(64)         | YES  | MUL | eqemu   |                |
| lsaccount_id        | int(11) unsigned    | YES  |     | NULL    |                |
| gmspeed             | tinyint(3) unsigned | NO   |     | 0       |                |
| invulnerable        | tinyint(4)          | YES  |     | 0       |                |
| flymode             | tinyint(4)          | YES  |     | 0       |                |
| ignore_tells        | tinyint(4)          | YES  |     | 0       |                |
| revoked             | tinyint(3) unsigned | NO   |     | 0       |                |
| karma               | int(5) unsigned     | NO   |     | 0       |                |
| minilogin_ip        | varchar(32)         | NO   |     |         |                |
| hideme              | tinyint(4)          | NO   |     | 0       |                |
| rulesflag           | tinyint(1) unsigned | NO   |     | 0       |                |
| suspendeduntil      | datetime            | YES  |     | NULL    |                |
| time_creation       | int(10) unsigned    | NO   |     | 0       |                |
| ban_reason          | text                | YES  |     | NULL    |                |
| suspend_reason      | text                | YES  |     | NULL    |                |
| crc_eqgame          | text                | YES  |     | NULL    |                |
| crc_skillcaps       | text                | YES  |     | NULL    |                |
| crc_basedata        | text                | YES  |     | NULL    |                |
+---------------------+---------------------+------+-----+---------+----------------+
```
  
</details>

<details>
<summary><strong>Click to view raw TAKP schema</strong></summary>
  
```sql
+-------------------------+---------------------+------+-----+---------------------+----------------+
| Field                   | Type                | Null | Key | Default             | Extra          |
+-------------------------+---------------------+------+-----+---------------------+----------------+
| id                      | int(11)             | NO   | PRI | NULL                | auto_increment |
| name                    | varchar(30)         | NO   | UNI | NULL                |                |
| charname                | varchar(64)         | NO   |     | NULL                |                |
| sharedplat              | int(11) unsigned    | NO   |     | 0                   |                |
| password                | varchar(50)         | NO   |     | NULL                |                |
| status                  | int(5)              | NO   |     | 0                   |                |
| lsaccount_id            | int(11) unsigned    | YES  | UNI | NULL                |                |
| forum_id                | int(10)             | NO   |     | 0                   |                |
| gmspeed                 | tinyint(3) unsigned | NO   |     | 0                   |                |
| revoked                 | tinyint(3) unsigned | NO   |     | 0                   |                |
| karma                   | int(5) unsigned     | NO   |     | 0                   |                |
| minilogin_ip            | varchar(32)         | NO   |     | NULL                |                |
| hideme                  | tinyint(4)          | NO   |     | 0                   |                |
| rulesflag               | tinyint(1) unsigned | NO   |     | 0                   |                |
| suspendeduntil          | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| time_creation           | int(10) unsigned    | NO   |     | 0                   |                |
| expansion               | tinyint(4)          | NO   |     | 12                  |                |
| ban_reason              | text                | YES  |     | NULL                |                |
| suspend_reason          | text                | YES  |     | NULL                |                |
| active                  | tinyint(4)          | NO   |     | 0                   |                |
| ip_exemption_multiplier | int(5)              | YES  |     | 1                   |                |
| gminvul                 | tinyint(4)          | NO   |     | 0                   |                |
| flymode                 | tinyint(4)          | NO   |     | 0                   |                |
| ignore_tells            | tinyint(4)          | NO   |     | 0                   |                |
| mule                    | tinyint(4)          | NO   |     | 0                   |                |
+-------------------------+---------------------+------+-----+---------------------+----------------+
```
  
</details>

### SQL Query Example
The following query in a TAKP schema will retrieve account data and add additional fields left as blank for eqemu.
```sql
SELECT 
    id,
    name,
    charname,
    '',                   -- auto_login_charname
    sharedplat,
    password,
    status,
    'local',              -- ls_id
    lsaccount_id,
    gmspeed,
    gminvul,              -- maps to invulnerable
    flymode,
    ignore_tells,
    revoked,
    karma,
    minilogin_ip,
    hideme,
    rulesflag,
    NULL,                 -- suspendeduntil (always NULL)
    time_creation,
    ban_reason,
    suspend_reason,
    NULL,                 -- crc_eqgame
    NULL,                 -- crc_skillcaps
    NULL                  -- crc_basedata
FROM account
WHERE name LIKE "soandso%";
```

## account_ip

> **ℹ️ Schema Compatibility**
> 
> The `account_ip` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### TAKP Format
```sql
+-----------+-------------+------+-----+---------------------+-------+
| Field     | Type        | Null | Key | Default             | Extra |
+-----------+-------------+------+-----+---------------------+-------+
| accid     | int(11)     | NO   | PRI | 0                   |       |
| ip        | varchar(32) | NO   | PRI | NULL                |       |
| count     | int(11)     | NO   |     | 1                   |       |
| lastused  | timestamp   | NO   |     | current_timestamp() |       |
+-----------+-------------+------+-----+---------------------+-------+
```

### PEQ Format
Same as TAKP (see above)

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
## character_faction_values
## character_inventory
## character_keyring
## character_languages
## character_memmed_spells
## character_skills
## character_spells