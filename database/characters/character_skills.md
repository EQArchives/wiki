---
title: character_skills
description: 
published: true
date: 2026-01-25T23:33:56.535Z
tags: database
editor: markdown
dateCreated: 2026-01-25T23:33:56.535Z
---

## character_skills

> **ℹ️ Schema Compatibility**
> 
> The `character_skills` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### Schema
```sql
+----------+-----------------------+------+-----+---------+----------------+
| Field    | Type                  | Null | Key | Default | Extra          |
+----------+-----------------------+------+-----+---------+----------------+
| id       | int(11) unsigned      | NO   | PRI | NULL    | auto_increment |
| skill_id | smallint(11) unsigned | NO   | PRI | 0       |                |
| value    | smallint(11) unsigned | NO   |     | 0       |                |
+----------+-----------------------+------+-----+---------+----------------+
```

**Primary Key**: `(id, skill_id)`

### Field Descriptions

- `id`: Character ID
- `skill_id`: Skill ID (0=1H Slashing, 1=2H Slashing, 2=Piercing, 3=1H Blunt, etc.)
- `value`: Current skill value (0-300+ depending on level cap and skill)

### Migration Query
```sql
SELECT 
    id,
    skill_id,
    value
FROM character_skills
WHERE id = 12345;  -- Replace with actual character_id
```