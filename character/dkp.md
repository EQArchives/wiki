---
title: DKP System
description: 
published: true
date: 2026-03-08T20:18:06.656Z
tags: dkp
editor: markdown
dateCreated: 2026-03-08T20:18:06.656Z
---

# DKP System Guide

## Table of Contents
- [What is DKP?](#what-is-dkp)
- [Member Guide](#member-guide)
- [Officer Guide](#officer-guide)
- [Admin Guide](#admin-guide)

---

## What is DKP?

DKP (Dragon Kill Points) is the system we use to fairly distribute loot during raids. Instead of random rolls, you earn points by showing up and killing mobs — then spend those points to win items you actually want.

The more you raid, the more DKP you earn. The more you bid, the more you spend. Your current DKP balance is always visible on your dashboard.

### Key Rules
- DKP is awarded **per mob killed** during a raid.
- All loot is distributed via **open auction** — you bid what you're willing to spend.
- The **minimum bid is 2 DKP**, with 1 DKP increments.
- There is a **65 DKP cap** on current DKP. This will increase as the player level cap increases.
- You can only bid DKP you currently have — not what you expect to earn.
- The first player to bid at a given amount wins that price point. Intentional ties are not permitted.
- If a player makes an all-in bid at the DKP cap, other raiders may also commit to an all-in bid. A roll-off determines the winner.
- You may spend DKP on items for any character you own, or a character you regularly bring to raids.

### Attendance
Your **90-day attendance percentage** is tracked automatically based on raids you attended in the past 90 days. It is displayed on your dashboard and the standings page.

---

## Member Guide

### Getting Started

#### Joining a Circuit
A **Raid Circuit** is a group of players who raid together and share a DKP pool. On many servers, this would usually be your Guild.  To join:

1. Go to **Raid DKP** in the site navigation.
2. Find the circuit you want to join (e.g. *Open Server Raids*) and click **Join**.
3. Enter a **display name** — this is the name other members will see. It does not have to match your account username.
4. Your request will be **pending** until an officer approves it.

> **Privacy note:** Your display name is the only identity visible to other members and officers. Your website account username is never shown.

#### Your Dashboard
Once approved, your dashboard is your home base. Find it under **Raid DKP → My Dashboard**. It shows:

- **Current DKP** — your spendable balance
- **Lifetime Earned** — total DKP ever awarded to you
- **Lifetime Spent** — total DKP you've spent on items
- **Attendance %** — your raid attendance over the past 90 days
- **Recent Transactions** — your last 20 DKP changes
- **Active Auctions** — open auctions you can bid on
- **My Bids** — your current active bids

#### Privacy
By default your dashboard is visible to other members. If you'd prefer to keep your DKP private, click the **👁 Visible** link next to your name to hide it. You can toggle this at any time.

---

### Standings
The **Standings** page shows all active members ranked by current DKP. You can filter by name using the search box.

---

### Auctions

#### How Auctions Work
When an item drops on a raid, an officer will create an auction for it. Auctions go through these states:

| Status | Meaning |
|--------|---------|
| Pending | Auction created, not yet open for bids |
| Open | Bidding is live |
| Closed | Bidding has ended, winner being determined |
| Awarded | Item has been awarded and DKP deducted |
| Disputed | Officer has flagged the auction for review |
| Retracted | Auction cancelled, any DKP spent has been refunded |

#### Placing a Bid
1. Go to **Raid DKP → Auctions** and find an open auction.
2. Click **View / Bid**.
3. Enter your bid amount and click **Place Bid**.
4. You can update your bid at any time while the auction is open.
5. You can retract your bid while the auction is open.
6. Once the auction closes, bids are locked.

> Bids are validated when the auction is awarded — you must have enough DKP at award time to cover your bid.

#### Bid Visibility
Whether other players can see bid amounts depends on the circuit's settings. Ask an officer if you're unsure.

---

### Raid History
You can browse past raids under **Raid DKP → Raid History**. Each raid shows:
- Attendees and their attendance status
- Mobs killed and DKP awarded
- Auctions from that raid

Private raids are only visible to circuit members.

---

### Transaction History
Your full DKP history is available under **My Dashboard → View All Transactions**. Each entry shows the date, type (award/spend/adjustment), item, amount, and any notes.

---

## Officer Guide

Officers manage raids, attendance, auctions, and circuit membership. Officer functions are available under the **Officer** section of the DKP sidebar.

### Managing Members

#### Approving New Members
When a player requests to join your circuit, you'll see a badge on **Manage Members** in the sidebar.

1. Go to **Manage Members**.
2. Click the **Pending** tab.
3. Review the request and click **Approve** or **Deny**.

#### Member Actions
On the **Active** tab you can:
- **Change role** — promote a member to Officer or demote to Member
- **Rename** — update a member's display name
- **Deactivate** — mark a member inactive (they keep their DKP history)
- **Remove** — permanently remove a member from the circuit

#### Inactive Members
Inactive members appear on the **Inactive** tab. You can reactivate them at any time.

---

### Managing Raids

#### Creating a Raid
1. Go to **Manage Raids → Create Raid**.
2. Set the date, labels (optional), and notes (optional).
3. Toggle **Private** if you want the raid hidden from non-members.
4. Click **Create**.

#### Managing Attendance
On the raid management page:
1. Check the box next to each member who attended.
2. Set their attendance status: **Present**, **Late**, or **Absent**.
3. Add notes if needed.
4. Click **Save Attendance**.

#### Managing Mobs
On the raid management page, select which mobs were killed during the raid and click **Save Mobs**. Mob DKP values are configured in **Circuit Settings**.

#### Awarding DKP
DKP is awarded per mob via the auction system. When an item drops:
1. Create an auction for the item on the raid page.
2. Open the auction when ready to take bids.
3. Close the auction when bidding ends — the system will automatically resolve the winner.
4. Award the auction to deduct DKP from the winner.

---

### Managing Auctions

#### Auction Actions
On the auction management page you can:
- **Open** — start accepting bids
- **Close** — end bidding and resolve winner
- **Award** — deduct DKP and mark item as awarded
- **Override Winner** — manually assign the item to a specific member (with optional reason)
- **Dispute** — flag the auction for review
- **Retract** — cancel the auction and refund any DKP spent

#### Override Winner
Use this when the automatic resolution needs to be corrected. You can specify a member, a custom DKP amount, and a reason. The previous winner (if any) will be automatically refunded.

---

### Circuit Settings
Go to **Circuit Settings** in the sidebar to configure:

| Setting | Description |
|---------|-------------|
| DKP Cap | Maximum current DKP a member can hold |
| DKP Overcap | How much over the cap a member can go temporarily |
| Minimum Bid | Lowest valid bid on any auction |
| New Player Bonus | Starting DKP bonus for new members |
| Hourly Rate | DKP awarded per hour for time-based raids |
| Attendance Window | Number of days used to calculate attendance % |
| Tie Breaker Rule | How ties in auctions are resolved |
| Public Bids | Whether bid amounts are visible to all members |

---