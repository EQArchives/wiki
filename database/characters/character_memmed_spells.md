---
title: character_memmed_spells
description: 
published: true
date: 2026-01-26T00:38:40.882Z
tags: database, takp_peq_migration
editor: markdown
dateCreated: 2026-01-26T00:38:40.882Z
---

## character_memmed_spells

> **ℹ️ Schema Compatibility**
> 
> The `character_memmed_spells` table is **identical** between PEQ and TAKP. No migration adjustments needed.
{.is-success}

### Schema
```sql
+----------+-----------------------+------+-----+---------+-------+
| Field    | Type                  | Null | Key | Default | Extra |
+----------+-----------------------+------+-----+---------+-------+
| id       | int(11) unsigned      | NO   | PRI | 0       |       |
| slot_id  | smallint(11) unsigned | NO   | PRI | 0       |       |
| spell_id | smallint(11) unsigned | NO   |     | 0       |       |
+----------+-----------------------+------+-----+---------+-------+
```

**Primary Key**: `(id, slot_id)`

### Field Descriptions

- `id`: Character ID
- `slot_id`: Spell gem slot (0-7 for the 8 memorized spell slots)
- `spell_id`: Spell ID currently memorized in that gem

### Spell Gem Slots

Classic EverQuest has **8 spell gems** for memorized spells:
- Slot 0: Spell gem 1
- Slot 1: Spell gem 2
- Slot 2: Spell gem 3
- Slot 3: Spell gem 4
- Slot 4: Spell gem 5
- Slot 5: Spell gem 6
- Slot 6: Spell gem 7
- Slot 7: Spell gem 8

> **ℹ️ Difference from character_spells**
> 
> - `character_spells`: Stores the entire spellbook (256 slots)
> - `character_memmed_spells`: Stores only the 8 currently memorized spells (spell bar)

### Migration Query
```sql
SELECT 
    id,
    slot_id,
    spell_id
FROM character_memmed_spells
WHERE id = 12345;  -- Replace with actual character_id
```