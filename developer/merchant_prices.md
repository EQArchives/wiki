---
title: Merchant Pricing
description: 
published: true
date: 2026-01-22T19:20:26.652Z
tags: merchant, pricing, developer, prices, vendor, price
editor: markdown
dateCreated: 2026-01-22T19:18:45.225Z
---

# Merchant Pricing

## Price Formula

When buying from a merchant, the final price is calculated as:

```
Final Price = item->Price × item->SellRate × CalcPriceMod(npc)
```

## Factors Affecting Price

### 1. CalcPriceMod()
**Location:** `zone/client.cpp:3023-3107`

The core price modifier is calculated based on:

- **Charisma** - Higher CHA results in better prices
- **Faction** - Amiable or better standing grants a +11 CHA bonus
- **NPC Greed** - Increases prices relative to the vendor's greed setting (default vendor markup is 25%)

### 2. SellRate
**Location:** `item_data.h:285`

A per-item multiplier stored in the item database that scales the final price.

### 3. NPC Greed
**Location:** `zone/npc.cpp:2569-2593`

- Can be fixed in the database via the `npc_types.greed` field
- Or dynamic if the `Merchant:UseGreed` rule is enabled (increases with shop_count)

**Example:** A price multiplier of 5.2x could result from `2600cp / 500cp = 5.2`

## Troubleshooting High Prices

### Possible Causes

1. **High SellRate** - The item may have an unusually high SellRate value (e.g., 5.2 instead of 1.0)
2. **NPC Greed** - The vendor may have a high greed value set in the database
3. **CalcPriceMod** - Combined effect of low Charisma and/or poor faction standing with vendor greed

## Database Queries

### Check Item SellRate

```sql
SELECT id, Name, Price, SellRate 
FROM items 
WHERE Name LIKE '%[item_name]%';
```

### Check NPC Greed

```sql
SELECT id, name, greed 
FROM npc_types 
WHERE name LIKE '%Nimren%Stonecutter%';
```