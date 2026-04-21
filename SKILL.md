---
name: pdw
description: Product Dev Workflow — a branch-per-task development assistant. Single slash command /pdw with seven sub-flows. Use /pdw alone to see the menu, or /pdw [sub] [args] for a specific flow. Sub-commands are init, talk, dev, hotfix, done, status. Also accepts natural language ("/pdw I want to build a login page") and auto-routes.
---

# /pdw — Product Dev Workflow

A branch-per-task development workflow. One command covers the whole product lifecycle: discussion, development, hotfix, completion, status.

## 🌐 Language Rule (applies to EVERYTHING below)

**Always reply in the same language the user writes in.** If the user writes Chinese, reply entirely in Chinese — translate all menus, prompts, confirmations, status messages, branch template section headers, and template placeholder labels. If the user writes English, reply entirely in English.

**Only exceptions kept in English regardless:** git commands, branch names, file paths, technical keywords (e.g. `main`, `HEAD`, `PR`, `CURRENT.md`).

Never mix languages unless the user does. The user's language is detected from their most recent message.

---

## Argument Parsing

When the user types `/pdw [args...]`, parse the first token:

| First token | Action |
| --- | --- |
| *(empty)* | Show the menu (see "Menu" section below) |
| `init` | Run the **Init** flow |
| `talk` (+ topic) | Run the **Roundtable** flow |
| `dev` (+ task) | Run the **Development** flow |
| `hotfix` (+ desc) | Run the **Hotfix** flow |
| `done` (+ `abandon`?) | Run the **Done** flow |
| `status` | Run the **Status** flow |
| Natural language description | Auto-route (see "Auto-Route" below) |

### Auto-Route (natural language)

If the first token is not a recognized sub-command, treat the whole argument as a natural-language description and auto-decide:

| Description signals | Route to |
| --- | --- |
| No `.claude/records/` exists yet | Suggest `init` first |
| Discussion / market analysis / ideation / "should we" / no dev needed | `talk` |
| Urgent / P0 / production down / broken / "now" / "asap" | `hotfix` |
| Feature / bug / refactor / batch / "build" / "add" / "fix" | `dev` |

Tell the user which sub-flow you picked and why, then proceed. If unsure, ask.

---

## Menu (shown when `/pdw` is typed with no args)

Display this menu in the user's language (translate headers, labels, and hints; keep command syntax in English):

```
📋 Product Dev Workflow — branch-per-task development

  /pdw                     Show this menu
  /pdw init                Initialize this project (one-time, any project)
  /pdw talk <topic>        Roundtable discussion (no dev)
  /pdw dev <task>          Development — new branch / resume / bug / batch
  /pdw hotfix <desc>       Emergency fix (auto-pauses current branch)
  /pdw done [abandon]      Finish current work
  /pdw status              Show current state (focus / paused / queued)

You can also describe your need in plain language:
  /pdw I want to add a login page
  /pdw analyze the Taiwan backup software market
  /pdw payment API is returning 500 in production

I will pick the right sub-flow for you.
```

---

## Sub-Flow 1: `init` — Initialize Project

**Model: 🟡 Sonnet**

Sets up `.claude/records/` structure for any project — empty or already in progress. Runs once per project.

### Pre-check

If `.claude/records/` already exists → abort and tell the user to run `/pdw status` instead.

### Detect Project Type

Run (or ask the user to run):
```
git branch --list
git branch --show-current
git log --oneline -1 2>/dev/null
git status --short
```

| Condition | Mode |
| --- | --- |
| No commits / no branches beyond main | 🟢 **Fresh Mode** |
| Has non-main branches or uncommitted work on a feature branch | 🔵 **Onboard Mode** |

If ambiguous (only `main` with commits but no other branches), ask the user to choose.

### 🟢 Fresh Mode

Create the structure without further questions:

```
.claude/records/
├── CURRENT.md
├── NOTES.md
├── branches/
└── archive/branches/
```

**CURRENT.md template** (translate labels to user's language):
```
# Current State

## Focus
(none)

## Paused
(none)

## Queued
(none)

## Version
current : v0.1
status  : not-started
```

Offer to add `.claude/` to `.gitignore`. Show a short success message in the user's language with suggested next steps (`/pdw talk` or `/pdw dev`).

### 🔵 Onboard Mode

For **each non-main branch**, ask the user (in their language):

1. Branch type: feature / bugfix / hotfix / refactor / unsure
2. One-line description of what this branch does
3. Current progress: planning / design done / mid-implementation / nearly done
4. Next action (one line — this is the resume point)

If the user can't remember a branch: offer "skip and mark as needs-review".

**Classify into CURRENT.md:**
- Current git branch → `Focus`
- Recent commits (≤7 days) → `Paused`
- Stale (>7 days no activity) → `Queued` with a "stale?" marker

Show the proposed state and confirm before writing files.

**Branch file (Snapshot Mode):** Generate from the appropriate template (light/medium/heavy by type — see Sub-Flow 3 for templates). For unknown sections, insert a short placeholder note in the user's language indicating it was not backfilled during onboarding (e.g. `*[not backfilled during onboarding]*` or `*[接入時未回溯]*`).

### Rules

- Read-only git queries only. Never run destructive git commands.
- Idempotent: second run aborts cleanly.
- Never force retroactive documentation.

---

## Sub-Flow 2: `talk` — Roundtable Discussion

**Model: 🟡 Sonnet**
**Purpose:** Market analysis, ideation, competitive research, architecture discussion — no development required.

### Before Starting

Prompt user to switch to Sonnet (in their language):
- Claude Code → `/model sonnet`
- Claude.ai → Click model name → Select Sonnet

### Fixed Roles (always present)

- 📋 Product Manager — requirements, priorities, alignment
- 🏗 Technical Architect — feasibility, architecture
- 👤 End User — pain points, usability

### Dynamic Roles (31 total — auto-select 2–4 by topic)

**C-Suite:** 👔 CEO · 🛠 CTO · 💼 COO · 💰 CFO · 🎨 CPO · 📣 CMO
**Management:** 🏗 Engineering Manager · 📊 Data Lead · 🔐 CISO · 🤝 Partnership Manager
**Specialists:** 🎨 UX Designer · ⚖️ Legal · 🧪 QA · 📊 Business Analyst · 📊 Data Engineer · 🔬 Data Scientist · 🔐 Security Engineer · 🏛 DevOps/SRE · 💰 Finance · 👤 Customer Success · 🎯 Growth · 🤝 Sales · 🌍 Localization · ♿ Accessibility · 📱 Mobile Engineer · 🤖 AI Engineer · 📣 Marketing
**External:** 🏢 Enterprise Client · 🔍 Competitor Analyst · 💡 Industry Advisor

### Auto-Selection by Topic

| Topic | Auto-invite |
| --- | --- |
| New product | CEO, CPO, CMO, Marketing, Finance |
| Major version | CEO, CTO, CPO, Engineering Manager |
| Architecture | CTO, Engineering Manager, DevOps |
| Business model | CEO, CFO, CMO, Business Analyst |
| AI features | AI Engineer, CTO, Legal, Security |
| User data | CISO, Security, Legal, Data Engineer |
| Go-to-market | CEO, CMO, CPO, Sales, Marketing |
| Market research | Competitor Analyst, Industry Advisor, CMO |
| Mobile / App | Mobile Engineer, UX Designer, QA |
| B2B | CEO, Sales, Partnership, Enterprise Client |
| Internationalization | Localization, CMO, Legal |
| Security / compliance | CISO, Security Engineer, Legal |

### Confirmation

Show proposed attendees and ask for confirmation (in user's language).

### Output Format

All section labels in user's language:

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

After output, ask:
- Save summary to `.claude/records/`?
- Start a development branch for any action item? (if yes, hand off to `dev` flow)

---

## Sub-Flow 3: `dev` — Development

**Model: 🟡 Sonnet** (🔴 Opus when stuck · 🔵 Haiku for batch)

### Decision Tree

```
/pdw dev [task?]
    ↓
Focus branch in CURRENT.md?
  YES + no new task   →  RESUME (show next step, continue)
  YES + new task      →  ASK ("Pause current and start new, or add to current?")
  NO                  →  NEW BRANCH
```

### Branch Classification (for NEW BRANCH)

| Signals | Type | Template | Model |
| --- | --- | --- | --- |
| rename / batch / list of ≥3 items | 🔵 batch | Light | Haiku |
| bug / error / crash / broken | 🟡 bugfix | Medium | Sonnet |
| refactor / cross-module / restructure | 🟢 refactor | Heavy | Sonnet/Opus |
| feature / add / build / new | 🟢 feature | Heavy | Sonnet |

### Confirmation Before Creating

Show (in user's language):
```
🌱 New branch
   name     : feature/login-page
   template : 🟢 heavy
   model    : 🟡 Sonnet
   git cmd  : git checkout -b feature/login-page
```

Ask to proceed / adjust / cancel.

### Branch Templates

When generating a branch file, translate all section headers to the user's language. Keep branch names, `main`, `HEAD` etc. in English.

**🔴 Light (hotfix / simple bug / batch)**
```
Type · Created · Base

Problem:
Fix:
Verification:
Tasks: [ ] Fix applied · [ ] Tested · [ ] Ready to merge
Pause Point:
  next step :
```

**🟡 Medium (bugfix)**
```
Type · Created · Base

Symptoms:
Root Cause:
Solution:
Tasks: [ ] Implement fix · [ ] Regression test · [ ] Docs
Test Plan:
PR Description (draft):
Pause Point:
  next step :
```

**🟢 Heavy (feature / refactor)**
```
Type · Created · Base

Background & Goal:
Design:
  Data / Schema:
  API / Interfaces:
  Flow / Sequence:
Task Breakdown:
  - [ ] Task 1
  - [ ] Task 2
Test Plan:
Risks & Open Questions:
PR Description (draft):
Pause Point:
  paused at :
  next step :
Notes:
```

### Resuming

When focus branch exists and no new task is given, show (in user's language):
- Branch name
- Last pause point + next step
- Task progress (X of Y done, with next task marked)
- Ask: continue, or show full branch file?

### Batch Mode (auto-triggered)

When classification → batch:
1. Show execution plan, ask for confirmation
2. Prompt Haiku switch
3. Execute, report results
4. Prompt switch back to Sonnet

### Blocker

If stuck: prompt Opus switch. User can continue on Sonnet if they prefer.

### During Development

If the user mentions a lesson learned, decision rationale, or gotcha → offer to save to `NOTES.md`.

---

## Sub-Flow 4: `hotfix` — Emergency Interrupt

**Model: 🟡 Sonnet** (🔴 Opus for mysterious bugs)

### Step 1 — Auto-Pause Current Focus

If `CURRENT.md` has an active focus:
1. Save pause point to the branch file:
   - `paused at:` — auto-detect from recent context if possible
   - `next step:` — ask the user for one line (in their language) to serve as the resume point
2. Move focus → paused in `CURRENT.md`.

### Step 2 — Open Hotfix Branch

```
git checkout -b hotfix/[short-desc]
```

Create branch file using the 🔴 Light template (headers in user's language).

### Step 3 — Assess and Fix

| Severity | Criteria | Approach |
| --- | --- | --- |
| P0 | Production down / data loss | Fix immediately, minimal docs |
| Hard | Can't identify root cause | Prompt Opus switch |
| Standard | Clear cause | Normal light-template flow |

### On Completion

When `/pdw done` closes this hotfix, offer to resume the paused focus branch. Show the saved `next step`. Ask in user's language.

---

## Sub-Flow 5: `done` — Finish Current Work

Context-aware — auto-detects what "done" means based on current state.

### Decision Tree

```
/pdw done [argument?]
    ↓
arg = "abandon"?                →  ABANDON
Focus is a hotfix branch?       →  CLOSE HOTFIX + offer resume
All tasks on focus checked?     →  CLOSE BRANCH normally
Tasks remaining?                →  Ask: which task done, or close whole branch?
No focus + version's branches done? →  Offer VERSION WRAP
No focus?                       →  Show status, suggest /pdw dev
```

### Close Branch (Normal)

Generate PR description from the branch file (in user's language). Confirm before archiving. Move `branches/[name].md` → `archive/branches/[name].md`. Clear focus in `CURRENT.md`. Show remaining queued branches or offer to resume paused.

### Close Hotfix

Same as normal close, then offer to resume the paused focus branch with its saved `next step`.

### Abandon

```
/pdw done abandon
```

Ask for optional reason. Archive with `status: abandoned`.

### Version Wrap (auto-offered)

When the last active branch of a version closes, offer wrap. User confirms with `wrap`.

**Retrospective flow (on wrap):**
1. Prompt Opus switch
2. Run retrospective with sections (in user's language): Period · Branches completed · Branches abandoned · What went well · What didn't · Key learnings · Tech debt noted · Next version ideas
3. Archive `CURRENT.md` snapshot to `archive/`, reset to empty template
4. Ask about planning the next version

---

## Sub-Flow 6: `status` — Show Current State

Read `CURRENT.md` and display in the user's language:

```
📊 Current State — [datetime]
────────────────────────────────

🎯 Focus
   [branch-name]   (started X days ago)
   Next: [next step]
   Tasks: X of Y done

⏸ Paused (N)
   [branch-name]
     paused X days ago
     next: "[resume instruction]"

🔜 Queued (N)
   [branch-name] — [note]

────────────────────────────────
Version: vX.x · N branches merged this version
```

If `.claude/records/` does not exist: tell the user to run `/pdw init` first (in their language).

Occasionally show a context hygiene reminder:
- Share with Claude: `CURRENT.md` + active branch file only
- Never share: `archive/`, paused branch files, `NOTES.md` (unless needed)

---

## Records Structure

```
project/
├── src/                        ← your code
├── .gitignore                  ← .claude/ added automatically by /pdw init
└── .claude/
    ├── skills/                 ← skill files
    └── records/
        ├── CURRENT.md          ← global state: focus / paused / queued
        ├── NOTES.md            ← lessons learned, decisions, gotchas
        ├── branches/           ← one file per active or paused branch
        └── archive/
            ├── branches/       ← merged / abandoned branches
            └── vX.X-*.md       ← version snapshots
```

---

## Model Quick Reference

```
🔴 Opus    → New product planning, major version, retrospective, hard bugs, architecture
🟡 Sonnet  → Roundtable, planning, development (main workhorse)
🔵 Haiku   → Batch tasks (auto-triggered via dev flow)
```

### How to Switch Models

- **Claude Code:** `/model opus` · `/model sonnet` · `/model haiku`
- **Claude.ai:** Click model name at top → select model

---

## Quick Examples

```
/pdw                                 → Show menu
/pdw init                            → Initialize this project
/pdw talk analyze Taiwan backup SaaS → Roundtable discussion
/pdw dev build a login page         → Open feature branch + planning
/pdw hotfix payment API returns 500 → Auto-pause + open hotfix branch
/pdw done                            → Close current branch
/pdw status                          → Where am I?

/pdw I want to add OAuth login       → Auto-routes to dev
/pdw production is on fire           → Auto-routes to hotfix
/pdw should we enter the EU market   → Auto-routes to talk
```
