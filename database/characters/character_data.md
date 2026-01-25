---
title: character_data
description: 
published: true
date: 2026-01-25T01:20:58.132Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T01:20:40.561Z
---

## character_data

## Tabs {.tabset}

### Schema Comparison

<div class="schema-summary">
  <div class="summary-card summary-peq">
    <h4>PEQ Exclusive (43 fields)</h4>
    <ul>
      <li>Drakkin customization (3 fields)</li>
      <li>Discipline abilities (4 fields)</li>
      <li>Leadership experience (4 fields)</li>
      <li>LDoN points (6 fields)</li>
      <li>Tribute system (4 fields)</li>
      <li>Extended PvP stats (7 fields)</li>
      <li>Group/raid consent (3 fields)</li>
      <li>Misc features (12 fields)</li>
    </ul>
  </div>
  
  <div class="summary-card summary-takp">
    <h4>TAKP Exclusive (0 fields)</h4>
    <ul>
      <li><code>forum_id</code></li>
      <li><code>boatid</code></li>
      <li><code>boatname</code></li>
      <li><code>famished</code></li>
      <li><code>is_deleted</code></li>
      <li><code>fatigue</code></li>
    </ul>
  </div>
  
  <div class="summary-card summary-shared">
    <h4>Compatible Fields</h4>
    <ul>
      <li>55 identical fields</li>
      <li>2 fields with differences</li>
      <li><strong>Core character data fully compatible</strong></li>
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
    <th>TAKP Type</th>
    <th>Migration Notes</th>
  </tr>
</thead>
<tbody>
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Core Identity Fields (9 fields) - All Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>id</code></td>
    <td>int(11) unsigned PRI AI</td>
    <td>int(11) unsigned PRI AI</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>account_id</code></td>
    <td>int(11) MUL</td>
    <td>int(11) MUL</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>forum_id</code></td>
    <td>—</td>
    <td>int(10)</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  <tr class="comparison-same">
    <td><code>name</code></td>
    <td>varchar(64) UNI</td>
    <td>varchar(64) UNI</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>last_name, title, suffix</code></td>
    <td colspan="3">All identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Position & Zone (5 fields) - Minor Differences</td>
  </tr>
  <tr class="comparison-same">
    <td><code>zone_id</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>zone_instance</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (no instances in TAKP)</td>
  </tr>
  <tr class="comparison-same">
    <td><code>x, y, z, heading</code></td>
    <td colspan="3">All float, identical</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Character Appearance (10 fields) - All Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>gender, race, class</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>face, hair_color, hair_style</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>beard, beard_color</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>eye_color_1, eye_color_2</code></td>
    <td colspan="3">All identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">Drakkin Customization - PEQ Only (3 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>drakkin_heritage</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (Drakkin race from TSS expansion)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>drakkin_tattoo, drakkin_details</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">Discipline Abilities - PEQ Only (4 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ability_time_seconds</code></td>
    <td>tinyint(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ability_number, ability_time_minutes, ability_time_hours</code></td>
    <td>tinyint(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Experience & Stats (20 fields) - All Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>level, deity, birthday</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>last_login, time_played, level2</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>anon, gm</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>exp</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>exp_enabled</code></td>
    <td>tinyint(1) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 1 (exp enabled by default)</td>
  </tr>
  <tr class="comparison-same">
    <td><code>aa_points_spent, aa_exp, aa_points</code></td>
    <td colspan="3">All identical</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">Leadership System - PEQ Only (4 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>group_leadership_exp</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (GoD expansion feature)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>raid_leadership_exp, group_leadership_points, raid_leadership_points</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Character Resources (13 fields) - All Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>points, cur_hp, mana, endurance</code></td>
    <td colspan="3">All identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>intoxication</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>str, sta, cha, dex, int, agi, wis</code></td>
    <td colspan="3">All int(11) unsigned, identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>extra_haste</code></td>
    <td>int(11)</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-same">
    <td><code>zone_change_count</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>toxicity</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-same">
    <td><code>hunger_level, thirst_level</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>famished</code></td>
    <td>—</td>
    <td>int(11)</td>
    <td><strong>TAKP only:</strong> Lost on migration (similar to hunger_level)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>fatigue</code></td>
    <td>—</td>
    <td>int(11)</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ability_up</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">LDoN System - PEQ Only (6 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ldon_points_guk, ldon_points_mir, ldon_points_mmc</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (LDoN expansion feature)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ldon_points_ruj, ldon_points_tak, ldon_points_available</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">Tribute System - PEQ Only (4 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>tribute_time_remaining</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (GoD expansion feature)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>career_tribute_points, tribute_points, tribute_active</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">PvP Stats (3 fields) - Shared</td>
  </tr>
  <tr class="comparison-same">
    <td><code>pvp_status</code></td>
    <td>tinyint(11) unsigned</td>
    <td>tinyint(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>pvp_kills, pvp_deaths</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>pvp_current_points, pvp_career_points</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>pvp_best_kill_streak, pvp_worst_death_streak, pvp_current_kill_streak</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>pvp2, pvp_type</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Display & UI Settings</td>
  </tr>
  <tr class="comparison-diff">
    <td><code>show_helm</code> / <code>showhelm</code></td>
    <td>int(11) unsigned, default 0</td>
    <td>tinyint(4), default 1</td>
    <td>Type and default differ - TAKP shows helm by default</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>xtargets</code></td>
    <td>tinyint(3) unsigned, default 5</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 5 (extended target window slots)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #e3f2fd; font-weight: bold;">Group/Raid Settings - PEQ Only (4 fields)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>group_auto_consent</code></td>
    <td>tinyint(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>raid_auto_consent, guild_auto_consent, leadership_exp_on</code></td>
    <td>tinyint(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Misc Settings & Flags</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>RestTimer</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-same">
    <td><code>air_remaining</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>autosplit_enabled</code></td>
    <td>int(11) unsigned</td>
    <td>int(11) unsigned</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>lfp</code></td>
    <td>tinyint(1) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (looking for party flag)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>lfg</code></td>
    <td>tinyint(1) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (looking for group flag)</td>
  </tr>
  <tr class="comparison-same">
    <td><code>mailkey</code></td>
    <td>char(16)</td>
    <td>char(16)</td>
    <td>Identical</td>
  </tr>
  <tr class="comparison-diff">
    <td><code>first_login</code> / <code>firstlogon</code></td>
    <td>int(11) unsigned</td>
    <td>tinyint(3)</td>
    <td>Field name and type differ but compatible</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>ingame</code></td>
    <td>tinyint(1) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (online status flag)</td>
  </tr>
  
  <tr class="comparison-same">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">AA Extended Data (5 fields) - All Identical</td>
  </tr>
  <tr class="comparison-same">
    <td><code>e_aa_effects, e_percent_to_aa, e_expended_aa_spent</code></td>
    <td colspan="3">All int(11) unsigned, identical</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>aa_points_spent_old, aa_points_old</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0 (legacy AA tracking)</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Boat/Mount System</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>boatid</code></td>
    <td>—</td>
    <td>int(11) unsigned</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>boatname</code></td>
    <td>—</td>
    <td>varchar(25) NULL</td>
    <td><strong>TAKP only:</strong> Lost on migration</td>
  </tr>
  
  <tr class="comparison-exclusive">
    <td colspan="4" style="background-color: #f5f5f5; font-weight: bold;">Deletion & Snapshot Tracking</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>is_deleted</code></td>
    <td>—</td>
    <td>tinyint(4)</td>
    <td><strong>TAKP only:</strong> Lost on migration (soft delete flag)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>deleted_at</code></td>
    <td>datetime NULL</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to NULL (soft delete timestamp)</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>e_last_invsnapshot</code></td>
    <td>int(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
  <tr class="comparison-exclusive">
    <td><code>illusion_block</code></td>
    <td>tinyint(11) unsigned</td>
    <td>—</td>
    <td><strong>PEQ only:</strong> Set to 0</td>
  </tr>
</tbody>
</table>

### Migration Considerations

> **✅ Good Compatibility**
> 
> Despite the large field count difference (106 vs 63), **all critical character data is compatible**:
> - Core stats, appearance, position, and experience transfer directly
> - The 43 PEQ-exclusive fields are primarily from post-classic expansions (LDoN, GoD, TSS)
> - The 6 TAKP-exclusive fields are minor (boat tracking, fatigue, forum_id)

> **🔄 Field Mappings**
> 
> Key mappings required:
> - `firstlogon` (TAKP) → `first_login` (PEQ) - field name and type differ
> - `showhelm` (TAKP) → `show_helm` (PEQ) - default value differs (1 vs 0)
> - Set all PEQ expansion fields to 0: LDoN points, tribute, leadership, drakkin
> - `zone_instance` set to 0 (TAKP doesn't use instances)
> - Handle `is_deleted` (TAKP) vs `deleted_at` (PEQ) for soft deletes

> **⚠️ Data Loss**
> 
> TAKP-specific fields that will be lost:
> - `forum_id` - Forum integration
> - `boatid`, `boatname` - Boat/mount tracking
> - `famished`, `fatigue` - Extended hunger/fatigue system
> - `is_deleted` - Soft delete flag (PEQ uses `deleted_at` timestamp instead)

> **📊 Expansion Features (PEQ Only)**
> 
> Set these to 0 - they're from expansions not in TAKP:
> - **Drakkin race** (TSS): drakkin_heritage, drakkin_tattoo, drakkin_details
> - **LDoN dungeons**: 6 ldon_points fields
> - **Leadership AA** (GoD): group/raid leadership exp and points
> - **Tribute system** (GoD): tribute points and timers

### Raw Schema Details

<details>
<summary><strong>Click to view raw PEQ schema (106 fields)</strong></summary>

```sql
[Full PEQ schema from document 6]
```

</details>

<details>
<summary><strong>Click to view raw TAKP schema (63 fields)</strong></summary>

```sql
[Full TAKP schema from document 7]
```

</details>

### SQL Query Example

Due to the large number of fields, here's a simplified query showing the pattern:

```sql
SELECT 
    -- Core identity (9 fields)
    id,
    account_id,
    name,
    last_name,
    title,
    suffix,
    
    -- Position (5 fields) 
    zone_id,
    0 AS zone_instance,  -- PEQ only
    x, y, z, heading,
    
    -- Appearance (10 fields)
    gender, race, class, level, deity,
    face, hair_color, hair_style, beard, beard_color,
    eye_color_1, eye_color_2,
    
    -- Drakkin (PEQ only - set to 0)
    0 AS drakkin_heritage,
    0 AS drakkin_tattoo,
    0 AS drakkin_details,
    
    -- Disciplines (PEQ only - set to 0)
    0 AS ability_time_seconds,
    0 AS ability_number,
    0 AS ability_time_minutes,
    0 AS ability_time_hours,
    
    -- Experience & progression
    birthday, last_login, time_played,
    level2, anon, gm,
    exp,
    1 AS exp_enabled,  -- PEQ only
    aa_points_spent, aa_exp, aa_points,
    
    -- Leadership (PEQ only - set to 0)
    0 AS group_leadership_exp,
    0 AS raid_leadership_exp,
    0 AS group_leadership_points,
    0 AS raid_leadership_points,
    
    -- Resources & stats
    points, cur_hp, mana, endurance, intoxication,
    str, sta, cha, dex, `int`, agi, wis,
    0 AS extra_haste,  -- PEQ only
    zone_change_count,
    0 AS toxicity,  -- PEQ only
    hunger_level, thirst_level,
    0 AS ability_up,  -- PEQ only
    
    -- LDoN (PEQ only - all 0)
    0 AS ldon_points_guk,
    0 AS ldon_points_mir,
    0 AS ldon_points_mmc,
    0 AS ldon_points_ruj,
    0 AS ldon_points_tak,
    0 AS ldon_points_available,
    
    -- Tribute (PEQ only - all 0)
    0 AS tribute_time_remaining,
    0 AS career_tribute_points,
    0 AS tribute_points,
    0 AS tribute_active,
    
    -- PvP (basic field shared, extended PEQ only)
    pvp_status,
    0 AS pvp_kills,
    0 AS pvp_deaths,
    0 AS pvp_current_points,
    0 AS pvp_career_points,
    0 AS pvp_best_kill_streak,
    0 AS pvp_worst_death_streak,
    0 AS pvp_current_kill_streak,
    0 AS pvp2,
    0 AS pvp_type,
    
    -- Display settings
    showhelm AS show_helm,  -- Note: TAKP default is 1, PEQ is 0
    0 AS group_auto_consent,
    0 AS raid_auto_consent,
    0 AS guild_auto_consent,
    0 AS leadership_exp_on,
    
    -- Misc
    0 AS RestTimer,
    air_remaining,
    autosplit_enabled,
    0 AS lfp,
    0 AS lfg,
    mailkey,
    5 AS xtargets,
    firstlogon AS first_login,
    0 AS ingame,
    
    -- AA extended
    e_aa_effects,
    e_percent_to_aa,
    e_expended_aa_spent,
    0 AS aa_points_spent_old,
    0 AS aa_points_old,
    0 AS e_last_invsnapshot,
    
    -- Deletion tracking
    NULL AS deleted_at,
    0 AS illusion_block
    
FROM character_data
WHERE id = 12345;  -- Replace with actual character_id
```

> **💡 Migration Tip**: This table is large but straightforward - most fields are 1:1 compatible. The main work is setting ~40 PEQ expansion fields to their default values (mostly 0).

This is a comprehensive comparison. The good news: **all the core character data (stats, appearance, position, experience) is fully compatible** between TAKP and PEQ!