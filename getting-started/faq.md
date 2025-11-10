---
title: Frequently Asked Questions
description: 
published: true
date: 2025-11-10T01:48:08.907Z
tags: getting-started, faq
editor: markdown
dateCreated: 2025-08-10T18:33:22.953Z
---

> Q: What are the ZEM values on this server? 

EQA's ZEM is currently synched with TAKP, and those values are listed here for easy reference: https://www.eqarchives.com/zones/ . Look for the ZEM tab on the left of that page.

---

> Q: Can bards do bard things here? 

A: Yes. There are no Bard song PBAE limits on this server.  However, there are zone pull limits.  This will remain this way as long as our bards respect their fellow players and don't create a nuisance that forces stricter limits.

---

>Q: What is the zone pull limit in zone x?

A: Zone pull limits are all set to 80 for Classic and Kunark zones.

---

>Q: What is considered a legacy item? Which version?

A: Legacy items remain on this server indefinitely at adjusted rarity.  We generally prefer the most powerful, non-nerfed version of the item to be available in-game, and they are listed at [Legacy Items](/world/legacy-items).

---

>Q: Are all items tradeable? 

A: Most items in-game are tradeable.  The exceptions are those associated with Epic Quests and Keys.  When keys are No Drop, they are generally Keyring enabled.  There is a list of keys and their properties at [Keyring](/character/keyring).

---

>Q: What third-party programs, afk behavior, or macros are permitted on this server? 

A: See [server_rules_conduct](/getting-started/server_rules_conduct).

---

>Q: Can anyone confirm the zone spawn timer for zone X?

A: The players app has a [Zones page](https://www.eqarchives.com/zones/) that provides the spawn timers for all spawn points in a given zone. Just select Zones at the top navbar, select a zone, and then click on the Spawns tab on the left.

---

>Q: How do I create a guild?

A: Contact a GM in-game or send them a DM in Discord to coordinate guild creation.  There are currently no restrictions provided the guild name is appropriate and not offensive (criteria for this will be left up to the GM's judgement call).

---

>Q: Why is feature X missing from the client?

A: It depends.  Some client features are gated to the expansion they were introduced on the original EQ Live timeline (like the Raid System and Luclin models), and others may not exist in the TAKP client if they were introduced after ~2002 when the client was built.

---

>Q: How do group experience bonuses work on this server?

A:  We have configured server rules to specifically incentivize groups larger than 3 (since this server permits a 3-box limit)
This is accomplished by setting two server rules: GroupExpMultiplier = 2 and GroupExpBonuses = True.  This results in a formula as follows:
`exp_per_character = ((mob_exp * groupmod) * GroupExpMultiplier) / group_member_count`. 
This is mostly self-explanatory except for the groupmod variable whose value depends on the number of group members as follows:
1  member = `((mob_exp * 1.0) * 1) / 1` = 100% experience
2 members = `((mob_exp * (1.0 + 0.2)) * 2) / 2` = 120% experience per group member
3 members = `((mob_exp * (1.0 + 0.4)) * 2) / 3` = 93% expperience per group member
4 members = `((mob_exp * (1.0 + 1.2)) * 2) / 4` = 110% experience per group member
5 members = `((mob_exp * (1.0 + 1.6)) * 2) / 5` = 104% experience per group member
6 members = `((mob_exp * (1.0 + 1.6)) * 2) / 6` = 87% experience per group member

To simplify this, reference the attached screenshot provided courtesy of @Deleted User .

For the technically inclined, the server source code that calculates group experience (where these formulas are derived) can be found at https://github.com/EQArchives/EQMacEmu/blob/24634bb8cab91286af32ad1023b8c518e66e1aa7/zone/exp.cpp#L802 .  Discussions of "GREEN_CON" mobs, group members being out of range, whether the 6th group member is counted, are out of scope of "group experience bonuses" and are determined by separate rules. As of this writing, the rules for ClassicGroupExpBonuses and VeliousGroupExpBonuses are disabled.

---

>Q: What are the settings on EQA for rez timers and corpse decay?

A: Corpses decay after 7 days when they have items on them, after 3 hours when the corpse is empty, and the rez timers are set to 3 hours.  You can check the current rez timer on a player corpse by targeting it and /consider.

This is a QoL improvement on the much more draconian historical Kunark release settings (take a look at p.34 of the Kunark Prima guide). On EQ Live at Kunark release, empty corpses used to decay within 3 minutes for all levels, 30 minutes for L1-5, and 24 hours while online for 6+ (character select counts), and 1 week while offline.