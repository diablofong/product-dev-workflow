---
name: product-dev-workflow
description: A branch-per-task product and software development workflow.
commands:
  - name: pdw
    description: "Universal entry — describe what you want, skill routes you"
  - name: pdw-init
    description: "One-time — initialize this project (empty or existing)"
  - name: pdw-talk
    description: "Roundtable discussion — market analysis, ideation, no dev"
  - name: pdw-dev
    description: "Development — new branch / resume / bug / batch auto-detect"
  - name: pdw-hotfix
    description: "Emergency fix — auto-pauses your current branch"
  - name: pdw-done
    description: "Finish current — complete / abandon / version wrap"
  - name: pdw-status
    description: "Show current state — focus / paused / queued"
---

# Product Development Workflow Skill

> **Activation rule:** This skill is activated ONLY when the user explicitly types one of the defined slash commands (`/pdw`, `/pdw-init`, `/pdw-talk`, `/pdw-dev`, `/pdw-hotfix`, `/pdw-done`, `/pdw-status`). Do NOT auto-trigger from conversation keywords, file contents, or any other context.

A branch-per-task, developer-friendly workflow for product and software development.

**Covers:** New products · New versions · Feature additions · Bug fixes · Hotfixes · Standalone discussions
**Language:** Claude replies in the same language the user writes in — no setup needed.

---

## Design Philosophy

- **One branch = one piece of work.** Each task gets its own git branch + its own planning document (BRANCH file).
- **Focus / Paused / Queued.** You work on one branch at a time, but interruptions (hotfix, review feedback) are first-class citizens with automatic pause/resume.
- **Context hygiene.** Claude only ever sees the current branch's file + `CURRENT.md`. Everything else is archived.
- **Seven commands total, six for daily use.** Four developer verbs — think (talk) · do (dev) · stop (done) · see (status) — plus a universal router, a dedicated interrupt, and a one-time onboarding command.

---

## Commands

| Command | Purpose |
| --- | --- |
| `/pdw [description]` | Universal entry — describe what you want, skill routes you |
| `/pdw-init` | **One-time** — initialize this project (empty or existing) |
| `/pdw-talk [topic]` | Roundtable discussion only — no development |
| `/pdw-dev [task]` | Development — auto-detects new branch / resume / bug / batch |
| `/pdw-hotfix [desc]` | Emergency — auto-pauses current branch |
| `/pdw-done` | Finish current — auto-detects complete / abandon / version wrap |
| `/pdw-status` | Show current state (focus / paused / queued) |

---

## /pdw — Universal Router

Parse the description and route to the right command.

### Pre-check: Is the project initialized?

Before any routing, check if `.claude/records/` exists:

- **Not initialized** → suggest `/pdw-init` first (works for both empty and existing projects). Don't block simple uses like `/pdw-talk` that don't need project state.
- **Initialized** → proceed with normal routing

### Routing rules

| Description contains | Route to |
| --- | --- |
| No dev required, market/ideation/discussion | `/pdw-talk` |
| Urgent / production down / hotfix / P0 | `/pdw-hotfix` |
| New feature / bug fix / refactor / batch task | `/pdw-dev` |
| No description | Show command menu |

### Without description

```
📋 Product Dev Workflow

  /pdw [description]   Universal — describe what you want
  /pdw-init            Onboard existing project (one-time)
  /pdw-talk            Roundtable discussion (no dev)
  /pdw-dev             Development (branch / resume / bug / batch auto-detect)
  /pdw-hotfix          Emergency fix (auto-pauses current branch)
  /pdw-done            Finish current work
  /pdw-status          Show current state

Not sure? Just type: /pdw [describe your need]
```

---

## /pdw-init — Initialize Project (One-Time)

**Model: 🟡 Sonnet**
**Purpose:** Set up this workflow on any project — whether brand new or already in progress. Runs once per project, then you never touch it again.

### When to use

- First time using this skill on this project (whether empty or existing)

### When NOT to use

- Project already has `.claude/records/` → run `/pdw-status` instead

### Flow

**Step 1 — Pre-check**

Check for `.claude/records/`:
- Exists → abort: "Already initialized. Use `/pdw-status` to see current state."
- Does not exist → proceed.

**Step 2 — Scan project state**

Run (or ask the user to run):

```
git rev-parse --is-inside-work-tree    # is it a git repo?
git branch --list                       # all local branches
git branch --show-current               # current branch
git log --oneline -1 2>/dev/null        # any commits?
git status --short                      # uncommitted changes?
```

Based on results, pick a mode:

| Condition | Mode |
| --- | --- |
| Not a git repo / no commits / no branches beyond main | 🟢 **Fresh Mode** |
| Has non-main branches OR uncommitted work on a feature branch | 🔵 **Onboard Mode** |

If uncertain (e.g. only `main` with existing commits but no feature branches), ask:

> This project has commits on `main` but no other branches. Treat as:
>   1. Fresh start — no backfill needed
>   2. Onboard — snapshot `main` work into a branch record
> Pick [1/2].

---

### 🟢 Fresh Mode — Empty / New Project

Simple, no questions asked beyond confirmation.

**Generate:**

```
.claude/
└── records/
    ├── CURRENT.md          ← empty template (no focus)
    ├── NOTES.md            ← empty template
    ├── branches/           ← empty directory (with .gitkeep)
    └── archive/
        └── branches/       ← empty directory (with .gitkeep)
```

**CURRENT.md (fresh):**

```markdown
# Current State

## Focus
(none — start with /pdw-dev or /pdw-talk)

## Paused
(none)

## Queued
(none)

## Version
current : v0.1
status  : not-started
```

**Then offer `.gitignore` update** (see Step 4 below).

**Output:**

```
✅ Project initialized (fresh mode).

Created:
  .claude/records/CURRENT.md
  .claude/records/NOTES.md
  .claude/records/branches/
  .claude/records/archive/branches/

.gitignore: .claude/ added

Ready to go. Try:
  /pdw                — Describe what you want to build
  /pdw-talk           — Discuss ideas first (recommended for new products)
  /pdw-dev [task]     — Jump straight into development
```

---

### 🔵 Onboard Mode — Existing Project with Branches

Same goal, more steps — snapshot your current work into the new structure.

**Step 3a — Interactive snapshot per branch**

For **each non-main/master branch**, ask:

```
🔍 Branch: feature/user-dashboard
   (current branch, has uncommitted changes)

  Type?
    1. feature (new functionality)
    2. bugfix
    3. hotfix
    4. refactor
    5. other / unsure

  [user picks]

  One-line description of what this branch is doing:
  > [user input]

  Current progress?
    1. Just started — planning phase
    2. Design done, starting implementation
    3. Mid-implementation
    4. Nearly done

  [user picks]

  What's the next action? (one line — this is your resume point)
  > [user input]
```

For branches the user doesn't remember clearly:

> Skip this branch? It'll be listed as "needs-review" and you can fill it in later.

**Step 3b — Classify focus vs paused vs queued**

- **Current git branch** → `focus` (unless user says otherwise)
- Branches with recent commits (last 7 days) → `paused`
- Stale branches (>7 days no activity) → `queued` with a "stale?" marker

Show the plan and confirm:

```
📋 Proposed state:

🎯 Focus
   feature/user-dashboard (current git branch)

⏸ Paused (1)
   bugfix/email-validation (active 2 days ago)

🔜 Queued (2)
   refactor/api-client (stale — 14 days)
   feature/notifications (stale — 21 days)

Type "ok" to proceed, or tell me what to adjust.
```

**Step 3c — Generate branch files in Snapshot Mode**

Use appropriate template (light/medium/heavy by type) but with `[待補 — 接入時未回溯]` markers for unknown sections.

**Example snapshot BRANCH file:**

```markdown
# feature/user-dashboard

**Type:** feature
**Created:** 2026-04-21 (initialized from existing branch)
**Status:** in-progress (nearly done)
**Base:** main

## Background & Goal
實作使用者儀表板的圖表與資料卡片

## Design
*[待補 — 接入時未回溯。若需討論再補上。]*

## Task Breakdown
- [x] Completed work (see git log for details — not backfilled)
- [ ] Connect weekly trend chart to real API  ← next

## Test Plan
*[待補]*

## Pause Point
paused at : (focus — actively working)
next step : Connect weekly trend chart to real API

## Notes
- Onboarded via /pdw-init on 2026-04-21
- Git branch already had 12 commits at onboarding
```

---

### Step 4 — `.gitignore` (both modes)

Check if `.gitignore` contains `.claude/`:
- Not present → append it (confirm with user)
- Present → skip

### Step 5 — Final output (both modes)

Fresh mode output: see above.

**Onboard mode output:**

```
✅ Project onboarded (onboard mode).

Files created:
  .claude/records/CURRENT.md
  .claude/records/NOTES.md
  .claude/records/branches/feature-user-dashboard.md  (🟢 heavy, snapshot)
  .claude/records/branches/bugfix-email-validation.md (🟡 medium, snapshot)
  .claude/records/branches/refactor-api-client.md     (🟢 heavy, snapshot)

.gitignore: .claude/ added

Current focus: feature/user-dashboard
  Next step: Connect weekly trend chart to real API

Suggested next actions:
  /pdw-status    — Verify everything looks right
  /pdw-dev       — Continue with your current focus
```

### Design notes

- **One command, two modes, auto-detected.** You don't need to pick — the skill reads git state and decides.
- **Never forces retroactive documentation.** Onboard mode's Snapshot Mode fills only what's known and flags the rest as deferred — you backfill during natural development, or never if the branch closes fast.
- **Respects your git state.** Read-only queries only (`git branch`, `git status`, `git log`). Never runs destructive commands.
- **Idempotent refusal.** Running `/pdw-init` twice aborts cleanly on the second run — no accidental overwrites.

---

## /pdw-talk — Roundtable Discussion

**Model: 🟡 Sonnet**
**Purpose:** Market analysis, ideation, competitive research, architecture discussion — independent of any branch or development plan.

### ⚠️ Before Starting — Switch Model

---

🟡 **Roundtable requires Sonnet**

- **Claude Code** → `/model sonnet`
- **Claude.ai Chat** → Click model name → Select Sonnet

Type **"continue"** when ready.

---

### Available Roles (Full List — 31 total)

**Fixed — always present:**

- 📋 Product Manager — requirements, priorities, alignment
- 🏗 Technical Architect — technical feasibility, architecture
- 👤 End User — real user pain points, usability

**C-Suite / Leadership:**

- 👔 CEO — company direction, vision, resource priorities
- 🛠 CTO — technology direction, tech debt, team capability
- 💼 COO — operations, execution efficiency
- 💰 CFO — financial risk, budget control
- 🎨 CPO — product strategy, user value, roadmap
- 📣 CMO — brand strategy, market entry

**Mid-level Management:**

- 🏗 Engineering Manager — engineering resources, scheduling
- 📊 Data Lead — data strategy, data governance
- 🔐 CISO — security strategy, compliance
- 🤝 Partnership Manager — partnerships, ecosystem

**Specialist Roles:**

- 🎨 UX Designer — user experience, flows
- ⚖️ Legal / Privacy — compliance, data protection
- 🧪 QA Engineer — quality, testing strategy
- 📊 Business Analyst — business logic, data, ROI
- 📊 Data Engineer — data architecture, pipelines
- 🔬 Data Scientist — analytics, ML feasibility
- 🔐 Security Engineer — security, vulnerabilities
- 🏛 DevOps / SRE — deployment, scalability, cost
- 💰 Finance — cost, pricing, ROI
- 👤 Customer Success — user retention, feedback
- 🎯 Growth — growth strategy, funnel
- 🤝 Sales — sales feasibility, customer feedback
- 🌍 Localization — multilingual, cultural fit
- ♿ Accessibility — inclusive design
- 📱 Mobile Engineer — mobile constraints
- 🤖 AI Engineer — AI feature feasibility
- 📣 Marketing — positioning, messaging

**External Perspectives:**

- 🏢 Enterprise Client — enterprise buying considerations
- 🔍 Competitor Analyst — competitive landscape, differentiation
- 💡 Industry Advisor — industry trends, regulations

### Auto-Selection Rules

Always include the 3 fixed roles. Then auto-select 2~4 dynamic roles:

| Topic Nature | Auto-invite |
| --- | --- |
| New product planning | CEO, CPO, CMO, Marketing, Finance |
| Major version (v2.0+) | CEO, CTO, CPO, Engineering Manager |
| Technical architecture | CTO, Engineering Manager, DevOps |
| Business model | CEO, CFO, CMO, Business Analyst |
| Data platform | CTO, Data Lead, Data Engineer, Data Scientist |
| AI features | AI Engineer, CTO, Legal, Security Engineer |
| Handles user data | CISO, Security Engineer, Legal, Data Engineer |
| Pre-launch review | QA Engineer, DevOps, Security Engineer, Marketing |
| Commercialization | CFO, Finance, Sales, Growth, Marketing |
| B2B product | CEO, Sales, Partnership Manager, Enterprise Client |
| Go-to-market | CEO, CMO, CPO, Sales, Marketing |
| Internationalization | Localization, CMO, Legal |
| Mobile / App | Mobile Engineer, UX Designer, QA Engineer |
| Security / compliance | CISO, Security Engineer, Legal |
| Market research | Competitor Analyst, Industry Advisor, CMO |

### Confirmation Before Starting

---

📋 **Suggested attendees for: [topic]**

Fixed:
- 📋 Product Manager
- 🏗 Technical Architect
- 👤 End User

Auto-selected:
- [Role] — [reason]
- [Role] — [reason]

Type **"ok"** to proceed, or adjust roles as needed.

---

### Auto Output

```
📄 ROUNDTABLE SUMMARY
────────────────────────────────
Topic      :
Attendees  :

[Role] Perspective:
  ...

Consensus      :
Action Items   :
Open Questions :
────────────────────────────────
```

After output:

> Save to `.claude/records/`? [yes / no]
> Start a branch for any action item? [yes — which one / no]

---

## /pdw-dev — Development

**Model: 🟡 Sonnet (switch to 🔴 Opus when stuck, 🔵 Haiku for batch)**

This is the **main workhorse command**. It auto-detects which development mode you need.

### Decision Tree

```
/pdw-dev [task?]
    ↓
┌─ Is there a focus branch in CURRENT.md? ─┐
│                                          │
├─ YES + no new description   →  RESUME   │
│   Read the branch file, show next step.  │
│                                          │
├─ YES + new description      →  ASK      │
│   "You have an active branch. Pause and  │
│    start new, or add this to current?"   │
│                                          │
└─ NO                         →  NEW      │
    Classify the description and open a    │
    new branch with the right template.    │
```

### Branch Classification

Parse the description for intent:

| Keywords / Signals | Branch Type | Template | Model |
| --- | --- | --- | --- |
| "rename", "批次", "for each", list of ≥3 items | 🔵 batch | Light | Haiku |
| "bug", "error", "crash", "壞了", "不會動" | 🟡 bugfix | Medium | Sonnet |
| "refactor", "重構", cross-module changes | 🟢 refactor | Heavy | Sonnet (Opus if architectural) |
| "feature", "add", "新增", "實作" | 🟢 feature | Heavy | Sonnet |
| Unclear | Ask | — | — |

### Opening a New Branch

```
🌱 Open a new branch for: [description]

Suggested:
  branch name : feature/oauth-refresh
  type        : 🟢 feature (heavy template)
  base        : main
  model       : 🟡 Sonnet

Proceed? [yes / adjust / cancel]
```

On confirmation:
1. Generate the git command: `git checkout -b feature/oauth-refresh`
2. Create `.claude/records/branches/feature-oauth-refresh.md` from the appropriate template
3. Update `CURRENT.md` focus section
4. Begin planning (for heavy template) or jump to execution (for light/medium)

### Branch Templates

#### 🔴 Light (hotfix / simple bug)

```markdown
# [branch-name]

**Type:** hotfix / simple bug
**Created:** YYYY-MM-DD HH:MM
**Base:** main

## Problem
[What's broken? How to reproduce?]

## Fix
[The change]

## Verification
[How to confirm it's fixed]

## Progress
- [ ] Fix applied
- [ ] Tested locally
- [ ] Ready to merge

## Pause Point (if interrupted)
paused at :
next step :
```

#### 🟡 Medium (standard bugfix)

```markdown
# [branch-name]

**Type:** bugfix
**Created:** YYYY-MM-DD
**Base:** main

## Symptoms
[What does the user see?]

## Root Cause
[Why it's happening]

## Solution
[The approach]

## Tasks
- [ ] Implement fix
- [ ] Add regression test
- [ ] Update docs if needed

## Test Plan
[How to verify + regression coverage]

## PR Description (draft)
[Fill when done]

## Pause Point
paused at :
next step :
```

#### 🟢 Heavy (feature / refactor)

```markdown
# [branch-name]

**Type:** feature / refactor
**Created:** YYYY-MM-DD
**Base:** main

## Background & Goal
[Why are we doing this? What's the user value?]

## Design
### Data / Schema
### API / Interfaces
### Flow / Sequence
### Dependencies

## Task Breakdown
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

## Test Plan
- Unit tests:
- Integration tests:
- Manual verification:

## Risks & Open Questions

## PR Description (draft)
[Fill when done]

## Pause Point
paused at :
next step :

## Notes
[Lessons learned during this branch]
```

### Batch Mode (auto-triggered)

When classification → batch:

---

🔵 **This is a batch task — switching to Haiku for lowest cost**

Execution plan:
[list of items and actions]

- **Claude Code** → `/model haiku`
- **Claude.ai Chat** → Click model name → Select Haiku

Type **"ok"** to proceed, or adjust the list.

---

After execution, report results and prompt:

---

🟢 **Batch complete — switch back to Sonnet**

- **Claude Code** → `/model sonnet`

---

### During Development

If a blocker is identified:

---

🔴 **Blocker detected — Consider switching to Opus**

- **Claude Code** → `/model opus`
- **Claude.ai Chat** → Click model name → Select Opus

Type **"continue"** or **"skip"** to stay on Sonnet.

---

If the user mentions something worth noting (lesson learned, decision rationale, gotcha):

> Note this to `NOTES.md`? [yes / no]

### Resuming a Paused Branch

When `/pdw-dev` is called with focus already set:

```
🔄 Resuming: feature/oauth-refresh

Last pause point:
  "Token refresh endpoint designed, about to implement"
Next step:
  "Implement POST /auth/refresh"

Current task list:
  [x] Design refresh flow
  [ ] Implement POST /auth/refresh  ← next
  [ ] Add refresh token to DB schema
  [ ] Write unit tests

Ready to continue? [yes / show full branch file]
```

---

## /pdw-hotfix — Emergency Interrupt

**Model: 🟢 Sonnet (🔴 Opus for mysterious hotfixes)**
**Purpose:** Something urgent broke — pause current work, fix, then resume.

### Flow

1. **Auto-pause current focus** (if any):

   ```
   ⏸ Pausing: feature/oauth-refresh

   Recording pause point…
     paused at  : [auto-detected from recent context]
     next step  : [auto-detected or ask user]

   Saved to branches/feature-oauth-refresh.md
   ```

2. **Confirm pause point** if auto-detection is uncertain:

   > What were you about to do next? (one line, for resume later)

3. **Open hotfix branch** with light template:

   ```
   🚨 Opening hotfix branch

   branch name : hotfix/payment-500
   base        : main (not current focus)
   model       : 🟢 Sonnet
   ```

4. **Assess severity** and proceed:

   | Severity | Criteria | Approach |
   | --- | --- | --- |
   | P0 hotfix | Production down / data loss | Fix immediately, minimal planning |
   | Hard hotfix | Cannot identify root cause | Switch to Opus |
   | Standard | Clear cause | Light template flow |

5. **Track the fix** in the hotfix branch file.

### On Completion

When `/pdw-done` is called on a hotfix branch, automatically offer:

> Hotfix merged. Resume **feature/oauth-refresh**? [yes / no / queue instead]

---

## /pdw-done — Finish Current Work

Context-aware. Auto-detects what "done" means based on current state.

### Decision Tree

```
/pdw-done [argument?]
    ↓
┌─ Focus is a hotfix branch?
│    → Close hotfix, offer to resume paused focus
│
├─ Focus is a regular branch + all tasks checked?
│    → Generate PR description, archive branch file, clear focus
│
├─ Focus is a regular branch + tasks remaining?
│    → Ask: "Mark which task done? Or close whole branch?"
│
├─ Argument is "abandon"?
│    → Mark branch abandoned, archive with reason
│
├─ No focus + all queued branches done for this version?
│    → Offer: "Version complete — wrap it up?" → retrospective
│
└─ No focus + nothing to close?
     → Show status, suggest /pdw-dev
```

### Closing a Branch (Normal Completion)

```
✅ Closing branch: feature/oauth-refresh

All tasks complete.

Generated PR description:
────────────────────────────────
## Summary
[auto-generated from branch file]

## Changes
- [task 1]
- [task 2]

## Testing
[from test plan]

## Related
[any linked issues]
────────────────────────────────

Type **"ok"** to archive this branch, or **"edit"** to modify PR description.
```

On confirmation:
1. Move `branches/feature-oauth-refresh.md` → `archive/branches/feature-oauth-refresh.md`
2. Clear focus in `CURRENT.md`
3. Show remaining queued branches or offer resume of paused

### Abandoning a Branch

```
/pdw-done abandon
```

```
🗑 Abandon branch: feature/oauth-refresh?

Reason (optional, for archive record):
> [user input]

Will archive with abandoned status.
```

### Version Wrap (auto-offered)

When the last branch of a version completes:

---

🎁 **All branches for v1.2 are done. Wrap this version?**

Will trigger retrospective (🔴 Opus recommended) + archive.

Type **"wrap"** to proceed, or **"skip"** to continue.

---

**Retrospective flow (on wrap):**

---

🔴 **Retrospective requires Opus**

- **Claude Code** → `/model opus`

Type **"continue"** when ready.

---

```
📄 RETROSPECTIVE — v1.2
────────────────────────────────
Period              :
Branches completed  :
Branches abandoned  :

What went well      :
What didn't         :
Key learnings       :
Tech debt noted     :
Next version ideas  :
────────────────────────────────
```

Archive step:
- Move `CURRENT.md` state → `archive/v1.2-current.md`
- Move `archive/branches/*` older than this version → `archive/v1.2/`
- Reset `CURRENT.md` to empty template

> ➡️ Start planning next version? [yes / no]

---

## /pdw-status — Show Current State

Read `CURRENT.md` and display everything in one view:

```
📊 Current State — 2026-04-21 14:32
────────────────────────────────────

🎯 Focus
   feature/oauth-refresh   (started 2d ago)
   Next: Implement POST /auth/refresh
   Tasks: 2 of 5 done

⏸ Paused (1)
   feature/subscription
     paused 3d ago
     next: "實作 Stripe webhook handler"

🔜 Queued (2)
   bugfix/login-session
   refactor/db-layer

────────────────────────────────────
Version: v1.2 in progress (3 branches merged)
```

**Context hygiene reminder** (shown occasionally):

> Only share `CURRENT.md` + the active branch file with Claude.
> Never share `archive/`, paused branch files, or `NOTES.md` unless specifically needed.

---

## Records Structure

```
project/
├── src/                              ← your code
├── .gitignore                        ← add .claude/ here
└── .claude/
    ├── skills/                       ← skill files
    └── records/
        ├── CURRENT.md                ← global state (focus/paused/queued)
        ├── NOTES.md                  ← lessons learned, decisions
        ├── branches/                 ← ALL active + paused branches
        │   ├── feature-oauth.md
        │   ├── feature-subscription.md  (paused)
        │   └── bugfix-login.md
        └── archive/
            ├── branches/             ← merged / abandoned branches
            │   └── feature-old.md
            ├── v1.0-current.md       ← version snapshots
            └── v1.0/                 ← branches belonging to that version
```

Add to `.gitignore`:

```
.claude/
```

### Context Hygiene Rules

- ✅ **Always share:** `CURRENT.md` + the current focus branch file
- 🔸 **Share when needed:** `NOTES.md` (for context on past decisions)
- ❌ **Never share:** `archive/`, paused branch files
- 💡 **Reference by path** when possible, don't paste entire files

---

## CURRENT.md Template

```markdown
# Current State

## Focus
→ [branch-name]
  type    : feature / bugfix / hotfix / refactor
  started : YYYY-MM-DD
  file    : branches/[branch-file].md
  next    : [one-line next action]

## Paused
⏸ [branch-name]
  paused  : YYYY-MM-DD
  reason  : [why paused, e.g. "hotfix interrupt"]
  file    : branches/[branch-file].md
  next    : [one-line pickup instruction]

## Queued
- [branch-name] — [brief note]

## Version
current : v1.2
status  : in-progress
merged  : [list of branches merged this version]
```

---

## Model Quick Reference

```
🔴 Opus    → New product planning, major version, retrospective, hard bugs, architecture
🟡 Sonnet  → Roundtable, feature/bugfix/refactor development (main workhorse)
🔵 Haiku   → Batch tasks (auto-triggered, developer doesn't need to pick)
```

**Rule of thumb:**

> Use Opus for decisions that affect the whole product.
> Use Sonnet for everything else.
> Batch tasks auto-route to Haiku — you don't need to think about it.

### How to Switch Models

**Claude.ai Chat**

> Click the model name at the top → Select the model

**Claude Code**

```
/model opus      # Switch to Opus
/model sonnet    # Switch to Sonnet
/model haiku     # Switch to Haiku
/model opusplan  # Opus for planning, auto-switch to Sonnet
```

---

## The Four Verbs

A mnemonic for daily use:

```
想 (think)  →  /pdw-talk       Discuss without coding
做 (do)     →  /pdw-dev        Write code in a branch
停 (stop)   →  /pdw-done       Finish what you're on
看 (see)    →  /pdw-status     Where am I?

+ /pdw-hotfix  for emergencies (auto-pauses current)
+ /pdw         when you don't know which to pick
+ /pdw-init    one-time, first use of this skill on any project (empty or existing)
```

---

## Flexible Usage

- You don't need to use every command. `/pdw-dev` alone covers most days.
- Branches are strongly recommended but not mandatory — for tiny throwaway edits you can skip the branch file.
- Heavy template is strongly recommended for any work expected to take >1 day or >100 lines of code.
- When in doubt: type `/pdw [describe what you want]` and the skill will guide you.
