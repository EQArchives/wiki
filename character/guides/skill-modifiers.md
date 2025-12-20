---
title: Skill Modifiers
description: Block, Dodge, Parry, Riposte modifiers
published: true
date: 2025-12-20T02:23:14.470Z
tags: guides
editor: markdown
dateCreated: 2025-12-20T02:23:14.470Z
---

# Skill Modifiers Guide
There exist modifiers for Block, Dodge, Parry, and Riposte skills in the game. The percentage is rounded down to nearest full number (integer) using this formula
```
// the divisor of 45 is for dodge
// change it to 50 for parry, 55 for riposte, 25 for block
chance = floor((skill+100 + floor((skill+100)*mod/100))/45)
```

This means you must reach the next full integer number to reach the next breakpoint. Sadly, this makes most modifiers available in the game useless since they don't improve your character's avoidance skills because the skill boost does not reach the next full number breakpoint.

---

## Breakpoint Reference

Based on this reference post: https://www.takproject.net/forums/index.php?threads/how-does-armor-currently-work-here.4224/page-5#post-86093

### Block skill cap at 65
- **230** - Monk - need 7% mod to reach next breakpoint
- **200** - Bst - need 9% mod to reach next breakpoint

### Dodge skill cap at 65
- **230** - Monk - need 10% mod to reach next breakpoint
- **210** - Rogue - need 2% mod to reach next breakpoint
- **190** - Warrior - need 9% mod to reach next breakpoint
- **170** - Pal/Rng/SK/Bard/Bst - need 17% mod to reach next breakpoint
- **75** - Clr/Dru/Shm/Nec/Wiz/Mag/Enc - need 3% mod to reach next breakpoint

### Parry skill cap at 65
- **230** - Warrior / Rogue - need 7% mod to reach next breakpoint
- **220** - Ranger - need 10% mod to reach next breakpoint
- **205** - Pal / SK - need 15% mod to reach next breakpoint
- **185** - Bard - need 6% mod to reach next breakpoint

### Riposte skill cap at 65
- **225** - Warrior / Monk / Rogue - need 2% mod to reach next breakpoint
- **200** - Pal / SK - need 10% mod to reach next breakpoint
- **185** - Bst - need 16% mod to reach next breakpoint
- **150** - Ranger - need 10% mod to reach next breakpoint
- **75** - Bard - need 26% mod to reach next breakpoint

## Notable Modifiers

Most do not matter. A few exceptions are:

- [Laminate Bracer](https://www.eqarchives.com/items/view/7469) for monks/BLs
- A few dodge mods for rogues/priests/casters
- [Ring of Reflex](https://www.eqarchives.com/items/view/7467) for warriors/rogues/bards
- [Bloodfrenzy](https://www.eqarchives.com/items/view/28855) for warriors
- [Mask of Resiliance](https://www.eqarchives.com/items/view/28931) or [Crystal Shadow Cloak](https://www.eqarchives.com/items/view/28918) for monks/rogues (and warriors without Bloodfrenzy)