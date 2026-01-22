---
title: Merchant Pricing
description: 
published: true
date: 2026-01-22T19:18:45.225Z
tags: merchant, pricing, developer, prices, vendor, price
editor: markdown
dateCreated: 2026-01-22T19:18:45.225Z
---

# Factors Affecting Merchant Pricing
### Key Price Formula                                                
When buying from a merchant:                                                                          
`Final Price = item->Price × item->SellRate × CalcPriceMod(npc)`

### Main Factors Affecting Price

1. **CalcPriceMod()** - zone/client.cpp:3023-3107                       

This is the core price modifier based on:

* Charisma - Higher CHA = better prices
* Faction - Amiable or better grants +11 CHA bonus
* NPC Greed - Increases prices (default vendor markup is 25%)                                                                                
2. **SellRate** - Item database field                                                         
A multiplier stored per item in the database (item_data.h:285)                                                                       
3. **NPC Greed** - zone/npc.cpp:2569-2593                                                 * Can be fixed in database (npc_types.greed field)
* Or dynamic if Merchant:UseGreed rule is enabled (increases with shop_count)                                                                 
The math: 2600cp / 500cp = 5.2x multiplier                                                                                                                                           
Possible causes:                                                                                                                          
1. SellRate in database - Check if the item has SellRate = 5.2 instead of 1.0                                                              
2. High NPC greed - Nimren Stonecutter may have a greed value set in npc_types                                                     
3. CalcPriceMod formula - With low CHA and/or bad faction, combined with greed, prices can inflate significantly                                                                                                                                   
### Quick Database Checks                                                                                                           
#### For the item:
```sql                                                                 
SELECT id, Name, Price, SellRate FROM items WHERE Name LIKE '%[item_name]%';               ```

#### For Nimren Stonecutter:

```sql
SELECT id, name, greed FROM npc_types WHERE name LIKE '%Nimren%Stonecutter%';
```