---
title: character_languages
description: 
published: true
date: 2026-01-25T18:42:27.778Z
tags: database, characters, takp_peq_migration
editor: markdown
dateCreated: 2026-01-25T18:42:27.778Z
---

## character_languages

> **ℹ️ Schema Compatibility**
> 
> The `character_languages` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### Schema
```sql
+---------+-----------------------+------+-----+---------+----------------+
| Field   | Type                  | Null | Key | Default | Extra          |
+---------+-----------------------+------+-----+---------+----------------+
| id      | int(11) unsigned      | NO   | PRI | NULL    | auto_increment |
| lang_id | smallint(11) unsigned | NO   | PRI | 0       |                |
| value   | smallint(11) unsigned | NO   |     | 0       |                |
+---------+-----------------------+------+-----+---------+----------------+
```

**Primary Key**: `(id, lang_id)`

### Field Descriptions

- `id`: Character ID
- `lang_id`: Language ID (0=Common Tongue, 1=Barbarian, 2=Erudian, 3=Elvish, etc.)
- `value`: Skill level in that language (0-100)

### Migration Query
```sql
SELECT 
    id,
    lang_id,
    value
FROM character_languages
WHERE id = 12345;  -- Replace with actual character_id
```