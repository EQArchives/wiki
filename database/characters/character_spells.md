---
title: character_spells
description: 
published: true
date: 2026-01-26T00:06:12.138Z
tags: database, takp_peq_migration
editor: markdown
dateCreated: 2026-01-26T00:06:12.138Z
---

## character_spells

> **ℹ️ Schema Compatibility**
> 
> The `character_spells` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### Schema
```sql
+----------+-----------------------+------+-----+---------+----------------+
| Field    | Type                  | Null | Key | Default | Extra          |
+----------+-----------------------+------+-----+---------+----------------+
| id       | int(11) unsigned      | NO   | PRI | NULL    | auto_increment |
| slot_id  | smallint(11) unsigned | NO   | PRI | 0       |                |
| spell_id | smallint(11) unsigned | NO   |     | 0       |                |
+----------+-----------------------+------+-----+---------+----------------+
```

**Primary Key**: `(id, slot_id)`

### Field Descriptions

- `id`: Character ID
- `slot_id`: Spellbook slot (0-255, with slots 0-7 being memorized spells)
- `spell_id`: Spell ID from the spells table

### Spell Slots

**Memorized spells (spell bar):**
- Slots 0-7: The 8 spell gems that can be cast

**Spellbook:**
- Slots 0-255: Complete spellbook storage (256 slots total)
- TAKP typically uses slots 0-255 (classic spellbook size)

### Migration Query
```sql
SELECT 
    id,
    slot_id,
    spell_id
FROM character_spells
WHERE id = 12345;  -- Replace with actual character_id
```