# Docket

> *One north star at a time.*

A personal task management webapp for professionals who are buried in work and need clarity, not complexity. Built as a single HTML file — no install, no account, no internet required after load. Everything lives on your device.

**by [@thedatacore](https://www.instagram.com/thedatacore)**

---

## The Problem

Most task apps treat all work equally. Your list grows. Nothing tells you what to do first. You context-switch constantly. Work piles up invisibly because there's no honest system forcing you to decide what actually matters right now.

Docket is built around a different philosophy: **triage over volume**. Before you work, you assess. Before you defer, you commit to a reason. Every decision leaves a trace.

---

## Core Principles

### 1 · One North Star at a Time

```
P3 ──▶ Your single most important task.
        Only ONE task can hold this slot.

        Assigning P3 to a new task?
        The current P3 auto-demotes to P2.
        Nothing is lost — it just steps down.
```

This forces clarity. You can never have two "most important" things.

---

### 2 · Deadline Overrides Priority

```
Sort Order:

  ┌─────────────────────────────────────────┐
  │  1. OVERDUE tasks (pinned to top)        │  ← Hard interrupt
  │     Sorted by: oldest overdue first      │
  ├─────────────────────────────────────────┤
  │  2. Active tasks with deadlines          │  ← Combined score
  │     Score = Priority weight              │
  │           + Deadline urgency weight      │
  ├─────────────────────────────────────────┤
  │  3. Tasks with no deadline               │  ← Priority only
  └─────────────────────────────────────────┘
```

A P1 task due in 6 hours will appear above a P3 task due next week. The list is always honest.

---

### 3 · Every Decision Leaves a Trace

Tasks are never quietly deleted or swiped away.

```
ACTIVE
  │
  ├──▶ COMPLETED    Timestamped. Log preserved.
  │
  └──▶ REJECTED     Mandatory remark required.
                    Full log preserved.
                    Can be reopened as a new task.
```

Your rejected list is a decision journal, not a trash bin.

---

## Priority System

| Level | Label       | Slot Limit | Behaviour                                      |
|-------|-------------|------------|------------------------------------------------|
| P3    | North Star  | **1 only** | Auto-demotes existing P3 to P2 on conflict     |
| P2    | Important   | Unlimited  | Former P3 tasks land here on demotion          |
| P1    | Background  | Unlimited  | Sorted by deadline proximity                   |

### The Demotion Cascade

```
BEFORE                          AFTER assigning new P3

  [P3] Fix settlement defect      [P3] Prepare client demo   ← New
  [P2] Write test cases           [P2] Fix settlement defect  ← Auto-demoted
  [P1] Update patch files         [P2] Write test cases
                                  [P1] Update patch files
```

You are notified every time a demotion happens. It is never silent.

---

## Task Anatomy

Every task carries the following fields:

```
┌─────────────────────────────────────────────────────┐
│  TITLE          Required. Short, action-oriented.    │
│  DESCRIPTION    Optional. Context and detail.        │
│  PRIORITY       1 · 2 · 3                           │
│  DEADLINE       Date + time. Triggers overdue logic. │
│  SOFT REVIEW    For open tasks — a revisit date.     │
│  CATEGORY       User-defined tag for grouping.       │
│  STATE          Active / Completed / Rejected        │
├─────────────────────────────────────────────────────┤
│  ACTIVITY LOG   Append-only timestamped notes.       │
│                 Written by you. Never editable.      │
│                 The task's living history.           │
├─────────────────────────────────────────────────────┤
│  METADATA       Created at · Updated at              │
│                 Completed at · Rejected at           │
│                 Rejection remark · Reopened from     │
└─────────────────────────────────────────────────────┘
```

---

## Task Lifecycle

```
                    ┌────────────────┐
                    │   NEW TASK     │
                    └───────┬────────┘
                            │
                            ▼
                    ┌────────────────┐
                    │    ACTIVE      │◀──────────────────┐
                    └───────┬────────┘                   │
                            │                            │
               ┌────────────┴────────────┐               │
               ▼                         ▼               │
      ┌─────────────────┐      ┌──────────────────┐      │
      │    COMPLETED    │      │    REJECTED      │      │
      │  timestamped    │      │  remark required │      │
      │  log preserved  │      │  log preserved   │      │
      └─────────────────┘      └────────┬─────────┘      │
                                        │                │
                                        │  Reopen as     │
                                        │  new task      │
                                        └────────────────┘
```

**Reopening a rejected task:**
- Creates a brand new Active task
- Pre-fills title, description, category
- First log entry reads: *"Reopened from rejected — original remark: [remark]"*
- Original rejected task remains in the Rejected list, unchanged
- New task starts at max P2 (cannot auto-inherit P3)

---

## Sort Score Logic

```
COMBINED SCORE = Priority Weight + Deadline Urgency Weight

Priority Weight:
  P3 → 7.5
  P2 → 5.0
  P1 → 2.5

Deadline Urgency Weight:
  Overdue         → +12  (pinned above all others)
  Due < 24h       → +9
  Due < 48h       → +6
  Due < 72h       → +4
  Due < 7 days    → +2
  Due > 7 days    → +1
  No deadline     → 0

Example:
  Task A: P2 (5.0) + due in 18h (+9)  = 14.0  ← appears first
  Task B: P3 (7.5) + due in 5 days (+2) = 9.5
  Task C: P1 (2.5) + OVERDUE (+12)   = 14.5  ← pinned above all
```

---

## Activity Log

Each task has its own append-only log — a timeline of what happened.

```
┌─────────────────────────────────────────────────────────┐
│ Mon 09:12   Reproduced in UAT. Scenario 4B confirmed.   │
│ Tue 15:30   Root cause — ID validation skipped on bulk. │
│ Wed 10:05   Fix applied. Running regression now.        │
│ Wed 14:00   Marked as complete.                         │
└─────────────────────────────────────────────────────────┘
```

- Entries are **timestamped on save**
- Entries **cannot be edited or deleted** — honest record only
- Completed and rejected tasks display the full log as read-only
- Multi-day tasks never lose context between sessions

---

## The Three Lists

### Active
Your working list. Sorted by combined score. Overdue tasks pinned to top with a banner.

```
  ⚠ 1 task past deadline — pinned for immediate attention

  ● Fix vendor BaNCS query            [overdue 2d]
  ● Fix settlement defect       P3    [due today]
  ● Write test cases            P2    [3 days]
  · Update patch files          P1    [4 days]
  · Investigate SWIFT failure   P2    [review Fri]
```

### Completed
Clean historical record. Reverse chronological. Full log accessible.

```
  ✓ Prepare DB migration scripts      [Tue 25 Mar, 12:04]
  ✓ Document client call action items [Mon 24 Mar, 16:45]
  ✓ Validate scenario 6A              [Wed 26 Mar, 13:10]
```

### Rejected
Decision journal. Mandatory remark visible on every card. Reopen action available.

```
  ✕ Validate scenario 9B              [Wed 26 Mar, 15:20]
    "Blocked — awaiting client test data. Cannot validate
     independently. Will follow up Monday."
    
    [ Reopen as new task ]
```

---

## Insights (Analysis)

The Insights tab reads your full task history and surfaces five signals. A **date range filter** applies to all charts simultaneously.

```
Date range:  [ 7 days ]  [ 30 days ]  [ 90 days ]  [ All time ]

┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  Completed  │  │  Rejected   │  │   Active    │
│      7      │  │      2      │  │      3      │
└─────────────┘  └─────────────┘  └─────────────┘
```

### Signal 1 — Completion by Priority
Are you actually finishing your P3 and P2 tasks, or just your P1 background work?

```
P3 · North Star   ████████████████░░░░   4/5
P2 · Important    ████████░░░░░░░░░░░░   3/7
P1 · Background   ████████████████████   3/3
```

### Signal 2 — Avg. Resolution Time
How long do tasks take from creation to completion by priority?

```
P3   ████████████   12h
P2   ████████████████████   28h
P1   ████   6h
```

If P2 consistently takes longer than P3, your priority assignments may not reflect actual urgency.

### Signal 3 — Completed by Category
Which types of work get done?

```
Defect       ████████████████   4
Client       ████████   2
Testing      ████████████   3
Internal     ████   1
```

### Signal 4 — Weekly Overdue Frequency
How often do tasks cross their deadline before resolution?

```
         │
    4 ── │        ▌
    3 ── │    ▌   ▌
    2 ── │    ▌   ▌   ▌
    1 ── │ ▌  ▌   ▌   ▌   ▌
         └────────────────────
          W1  W2  W3  W4  W5
```

A rising pattern here signals either deadline estimation is off or the priority system isn't being used honestly.

### Signal 5 — Rejection Log
Your full rejection history with remarks. Patterns become visible over time:
- Repeated "blocked by client" → upstream dependency problem
- Repeated "overcommitted" → estimation problem
- Repeated "deprioritised" → scope problem

---

## Category Management

Three default categories ship with Docket:

| Category   | Deletable | Renameable |
|------------|-----------|------------|
| Defect     | No        | Yes        |
| Review     | No        | Yes        |
| Follow-up  | No        | Yes        |

Custom categories can be added, renamed, and deleted freely from Settings. If a custom category is deleted, existing tasks retain the category name as a plain text label.

---

## Data Export

Export your full task history at any time from Settings.

```json
{
  "exportedAt": "2025-03-24T09:00:00.000Z",
  "version": "docket_v2",
  "by": "@thedatacore",
  "tasks": [
    {
      "id": "m7x3kp9a",
      "title": "Fix duplicate business partner defect",
      "description": "Reproduces on scenario 4B and 7C in UAT.",
      "priority": 3,
      "deadline": 1743019200000,
      "softReview": null,
      "category": "Defect",
      "state": "completed",
      "log": [
        { "ts": 1742900520000, "text": "Reproduced in UAT. 4B confirmed." },
        { "ts": 1742985300000, "text": "Root cause identified. Fix in progress." }
      ],
      "createdAt": 1742889600000,
      "completedAt": 1743009660000,
      "rejectionRemark": null,
      "reopenedFrom": null
    }
  ],
  "categories": ["Defect", "Review", "Follow-up", "Release"]
}
```

- All fields included: logs, remarks, timestamps, reopen references
- One JSON file per export, named `docket-export-YYYY-MM-DD.json`
- No import in MVP — export is for backup and data portability

---

## Technical Details

| Property        | Value                              |
|-----------------|------------------------------------|
| Format          | Single `.html` file                |
| Dependencies    | Google Fonts (loaded on first open) |
| Framework       | Vanilla JS — no build tool         |
| Persistence     | `localStorage` (device-only)       |
| Offline         | Fully offline after first load     |
| PWA             | Yes — add to home screen           |
| Theme           | Dark (zen, minimal)                |
| Max width       | 480px, mobile-first                |

### Running Locally

```bash
# No build step needed.
# Just open the file in any modern browser.

open docket.html           # macOS
start docket.html          # Windows
xdg-open docket.html       # Linux
```

### Installing as a Mobile App

**iOS (Safari)**
1. Open `docket.html` in Safari
2. Tap the Share icon
3. Tap *Add to Home Screen*
4. Name it **Docket** → Add

**Android (Chrome)**
1. Open `docket.html` in Chrome
2. Tap the three-dot menu
3. Tap *Add to Home Screen*
4. Confirm

The app will behave like a native app with no browser chrome.

### Data Storage

All data is stored in `localStorage` under the key `docket_v2`. The schema is versioned to allow future migrations without data loss.

```
localStorage
  └── docket_v2
        ├── tasks[]         All tasks across all states
        ├── categories[]    User-defined category list
        └── onboarded       Boolean — first-launch flag
```

**Important:** `localStorage` is scoped to the origin (file path or domain). Data on one device does not sync to another. Use Export to back up or transfer.

---

## Roadmap — Next Phase

The MVP is intentionally scoped to single-user, local-only. The planned production version introduces:

| Layer          | Technology                              |
|----------------|-----------------------------------------|
| Frontend       | React Native (mobile) + React (web)     |
| Backend        | Java + Spring Boot microservices        |
| Database       | PostgreSQL                              |
| Auth           | JWT-based authentication system         |
| Cloud          | AWS (hosting + storage)                 |
| Containers     | Docker + Kubernetes                     |
| AI Layer       | LLM integration for task auto-fill      |

**AI integration vision:** User provides minimum information — a sentence, a voice note, a pasted message. The AI infers title, description, category, suggested priority, and suggested deadline. User confirms or adjusts before saving. The goal is to remove the friction of structured data entry without removing human control over the final record.

---

## Philosophy

Docket was designed for the professional who already knows what good work feels like — they just need a system that doesn't get in the way.

> **No subscriptions. No syncing. No notifications. No teams. No roadmaps.**  
> Just you, your tasks, and an honest list of what happened to them.

The zen aesthetic is intentional. Your task manager should be a calm space — not a dashboard anxiety machine. When you open Docket, the only question on screen is: *what matters right now?*

---

## License

MIT — do whatever you want with it.

---

*Docket MVP · built by [@thedatacore](https://www.instagram.com/thedatacore)*
