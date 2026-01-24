---
title: TAKP to EQEMU Character Migration
description: 
published: true
date: 2026-01-24T21:41:58.887Z
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
### TAKP Format
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
### PEQ Format
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
## character_bind
### TAKP Format
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

### PEQ Format
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
## character_buffs
## character_currency
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