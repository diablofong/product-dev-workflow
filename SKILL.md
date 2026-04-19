---
name: product-dev-workflow
description: "A flexible product and software development workflow skill. Activated ONLY via slash commands (/pdw, /pdw roundtable, /pdw dev, /pdw bug, /pdw batch, /pdw done, /pdw status, /pdw wrap). Do NOT auto-trigger from conversation keywords, file contents, or any other context. Only load this skill when the user explicitly types one of the above slash commands."
---

# Product Development Workflow Skill

A streamlined, developer-friendly workflow for product and software development.
Supports: New products · New versions · Feature additions · Bug fixes · Standalone discussions
Language: Claude replies in the same language the user writes in — no setup needed.

---

## Commands

| Command | When to Use |
|---|---|
| `/pdw [description]` | Universal entry — describe what you want, Skill figures out the rest |
| `/pdw` | No idea where to start — show the menu |
| `/pdw roundtable [topic]` | Standalone discussion — market analysis, ideation, no dev required |
| `/pdw dev [task]` | Start a development task |
| `/pdw bug [description]` | Fix a bug or hotfix |
| `/pdw batch [description]` | Batch repetitive tasks — rename, update, annotate, replace |
| `/pdw done [task]` | Mark a task complete + update records |
| `/pdw status` | Show current progress |
| `/pdw wrap` | Archive current version + retrospective |

---

## /pdw — Universal Entry Point

### With description: `/pdw [description]`

Analyze the description and automatically determine:

1. **What type of task is this?**

| Type | Criteria | Entry |
|---|---|---|
| New product | Brand new, no existing codebase | Phase 1 |
| New version | Major changes to existing product | Phase 1 |
| Major feature | Affects architecture or business model | Phase 1 or 2 |
| New feature | Standard addition, no major impact | Phase 3 |
| Development task | Clear spec already exists | Phase 4 |
| Discussion only | No development needed | Roundtable |

2. **Does this need a roundtable?**

Suggest roundtable if ANY of these are true:
- Affects product direction or business model
- Involves multiple departments or roles
- Has significant risk or uncertainty
- Requires stakeholder alignment

Display suggestion and wait for confirmation:

---
💡 **Suggested: Open a roundtable first**

Reason: [explain why]

Type **"yes"** to open roundtable first, or **"skip"** to proceed directly.

---

3. **Which model to use?**

Display model recommendation and wait for user to switch:

---
🔴 **This task requires Opus**

- **Claude Code** → `/model opus`
- **Claude.ai Chat** → Click model name → Select Opus

Type **"continue"** when ready.

---

### Without description: `/pdw`

Display the command menu:

```
📋 Product Dev Workflow

  /pdw [description]     Universal entry — just describe what you want
  /pdw roundtable        Standalone discussion / market analysis
  /pdw dev               Development task
  /pdw bug               Bug fix or hotfix
  /pdw batch             Batch repetitive tasks
  /pdw done              Mark task complete
  /pdw status            Current progress
  /pdw wrap              Archive this version

Not sure? Just type: /pdw [describe your need]
```

---

## /pdw roundtable — Standalone Discussion

**Model: 🟡 Sonnet**
**Purpose: Market analysis, ideation, competitive research, architecture discussion — independent of any development plan**

### ⚠️ Before Starting — Switch Model

---
🟡 **Roundtable requires Sonnet**

- **Claude Code** → `/model sonnet`
- **Claude.ai Chat** → Click model name → Select Sonnet

Type **"continue"** when ready.

---

### Available Roles (Full List)

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
|---|---|
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

Consensus  :
Action Items:
Open Questions:
────────────────────────────────
```

After output, ask:
> Save this summary to `.claude/records/`? [yes / no]

---

## Development Phases

### Phase 1 — Product Planning & Market Evaluation
**Model: 🔴 Opus**
**Triggered by: /pdw [new product or major version]**

### ⚠️ Switch Model

---
🔴 **Phase 1 requires Opus**

- **Claude Code** → `/model opus`
- **Claude.ai Chat** → Click model name → Select Opus

Type **"continue"** when ready.

---

**Work Items:**
- Define core pain point and user problem
- Identify target users
- Competitive analysis
- Differentiation strategy
- Business model
- Initial roadmap
- Technical feasibility overview

**Auto Output:**
```
📄 PRODUCT BRIEF
────────────────────────────────
Product Name    :
Core Pain Point :
Target Users    :
Competitors     :
Differentiation :
Business Model  :
Initial Roadmap :
Open Questions  :
────────────────────────────────
```

> ➡️ Open roundtable next? [yes / skip]
> ➡️ Proceed to execution planning? [yes / no]

---

### Phase 2 — Roundtable (if needed)
See `/pdw roundtable` above. Same process, triggered automatically when needed.

> ➡️ Proceed to execution planning? [yes / no]

---

### Phase 3 — Execution Planning
**Model: 🟢 Sonnet**

Select documents to produce:
- [ ] PRD — Product Requirements Document
- [ ] Technical Specification
- [ ] Roadmap
- [ ] UI/UX Flow
- [ ] Test Plan
- [ ] Marketing Plan

After producing selected documents, save to `PLAN.md` and generate `CURRENT.md` with the task list.

**Auto Output:**
```
📄 EXECUTION PLAN SUMMARY
────────────────────────────────
Documents Produced :
Key Milestones     :
Dependencies       :
Risks              :
────────────────────────────────
```

> ➡️ Start development? [yes / no]

---

### Phase 4 — System Development
**Model: 🟢 Sonnet (switch to 🔴 Opus when stuck)**

| Task Type | Model |
|---|---|
| Feature implementation | 🟢 Sonnet |
| Writing tests | 🟢 Sonnet |
| Refactoring | 🟢 Sonnet |
| Known bug fix | 🟢 Sonnet |
| Hard / mysterious bug | 🔴 Opus |
| Cross-module refactor | 🔴 Opus |
| Architecture decision | 🔴 Opus |
| Batch tasks (uses Haiku internally) | 🔵 Haiku → use `/pdw bug` or direct |

When a blocker is identified:

---
🔴 **Blocker detected — Consider switching to Opus**

- **Claude Code** → `/model opus`
- **Claude.ai Chat** → Click model name → Select Opus

Type **"continue"** or **"skip"** to stay on Sonnet.

---

During development, if the user mentions something worth noting (a lesson learned, a decision rationale, a discovered gotcha), automatically offer:
> Note this to `NOTES.md`? [yes / no]

---

### Phase 5 — Next Version Planning
**Model: 🔴 Opus**
Triggered by `/pdw wrap` completing. Return to Phase 1 with current product as context.

---

## /pdw dev — Development Task

**Model: 🟢 Sonnet**

1. Read `CURRENT.md` to understand the task list
2. Ask which task to work on (or accept from command)
3. Execute Phase 4 development flow
4. After completion, prompt: "Mark as done? Type `/pdw done [task]`"

For batch mechanical tasks (no logic needed):

---
🔵 **This looks like a batch task — consider Haiku**

- **Claude Code** → `/model haiku`

Type **"continue"** or **"skip"** to stay on Sonnet.

---

---

## /pdw bug — Bug Fix & Hotfix

**Model: 🟢 Sonnet (switch to 🔴 Opus for hard bugs)**

1. Understand the bug (symptoms, when it occurs, expected vs actual)
2. Assess severity:

| Severity | Criteria | Approach |
|---|---|---|
| Hotfix | Production down / data loss | Fix immediately, skip planning |
| Standard | Known bug, clear cause | Normal fix flow |
| Hard bug | Cannot identify root cause | Switch to Opus |

3. Hypothesize root cause
4. Propose fix with code
5. Explain how to verify
6. Suggest regression test

For hard bugs:

---
🔴 **Consider switching to Opus for this bug**

- **Claude Code** → `/model opus`

Type **"continue"** or **"skip"** to stay on Sonnet.

---

---

## /pdw done — Mark Complete

1. Find the task in `CURRENT.md`
2. Move it to `DONE.md` with completion date:
   ```
   - [x] Task name — completed 2026-04-19
   ```
3. Update `CURRENT.md`
4. Show remaining tasks

---

## /pdw status — Current Progress

Read `CURRENT.md` and display:

```
📊 CURRENT PROGRESS
────────────────────────────────
Project  :
Version  :

In Progress:
  [ ] Task A
  [ ] Task B

Blocked:
  [ ] Task C — reason

Upcoming:
  [ ] Task D
────────────────────────────────
X of Y tasks remaining
```

Also remind:
> Only share `CURRENT.md` with Claude — not `DONE.md`, `PLAN.md`, or `archive/`

---

## /pdw wrap — Archive & Retrospective

**Model: 🔴 Opus (for retrospective)**

### Step 1 — Retrospective

---
🔴 **Retrospective requires Opus**

- **Claude Code** → `/model opus`

Type **"continue"** when ready.

---

Conduct a brief retrospective:

```
📄 RETROSPECTIVE
────────────────────────────────
Version    :
Period     :

What went well    :
What didn't       :
Key learnings     :
Tech debt noted   :
Next version ideas:
────────────────────────────────
```

### Step 2 — Archive

Confirm before archiving:

---
📦 **Ready to archive?**

Will move:
- `DONE.md` → `archive/vX.X-done.md`
- `PLAN.md` → `archive/vX.X-plan.md`

Type the version number (e.g. **"v1.0"**) to confirm, or **"cancel"** to abort.

---

After archiving, reset `CURRENT.md` and `DONE.md` to empty templates.

> ➡️ Start planning next version? [yes / no]

---

## /pdw batch — Batch Repetitive Tasks

**Model: 🔵 Haiku (lowest cost — used internally)**
**Purpose: Any task that repeats the same action across multiple items, requires no logic or judgment**

> 💡 You don't need to know about Haiku. Just use `/pdw batch` and the Skill handles the model selection automatically.

### Suitable Tasks

- Rename variables, functions, or files across a codebase
- Add comments or annotations to a list of functions
- Run the same method against a list of IDs or items
- Replace specific strings batch-wide
- Generate boilerplate from a fixed pattern
- Format or reformat code consistently

### Not Suitable (use `/pdw dev` instead)

- Each item needs individual judgment
- Involves business logic or rules
- Requires understanding context per item

### Usage

```
/pdw batch [describe the task and provide the list]
```

### Examples

```
/pdw batch rename these functions to camelCase: [list]
/pdw batch add error log to these functions: [list]
/pdw batch call updateStatus() for each: id:001→active, id:002→inactive
/pdw batch add JSDoc comments to these: [list]
```

### Flow

1. Analyze the task — confirm it is suitable for batch execution
2. If not suitable, suggest `/pdw dev` instead and explain why
3. Break down the task into a clear execution list
4. Display the list and wait for confirmation:

---
📋 **Batch execution plan:**

[list of items and actions]

Type **"ok"** to proceed, or adjust the list.

---

5. Switch to Haiku:

---
🔵 **Switching to Haiku for batch execution (lowest cost)**

- **Claude Code** → `/model haiku`
- **Claude.ai Chat** → Click model name → Select Haiku

Type **"continue"** when ready.

---

6. Execute and report results in the most readable format for the task:

```
Batch rename:
✅ getUserInfo() → fetchUserProfile() (3 files)
✅ setData() → updateData() (1 file)

Batch method call:
✅ updateStatus(001) → active
✅ updateStatus(002) → inactive
❌ updateStatus(003) → failed (reason)

Summary: X items — Success: X, Failed: X
```

7. After completion, prompt to switch back:

---
🟢 **Batch complete — switch back to Sonnet**

- **Claude Code** → `/model sonnet`
- **Claude.ai Chat** → Click model name → Select Sonnet

---

---

## Records Structure

```
project/
├── src/                        ← your code
├── .gitignore                  ← add .claude/ here
└── .claude/
    ├── skills/                 ← skill files
    └── records/
        ├── CURRENT.md          ← active tasks (only this goes in context)
        ├── DONE.md             ← completed items (never share with Claude)
        ├── PLAN.md             ← full plan (reference only)
        ├── NOTES.md            ← lessons learned, decisions, gotchas
        └── archive/
            ├── v1.0-done.md
            └── v1.0-plan.md
```

Add to `.gitignore`:
```
.claude/
```

**Context hygiene rules:**
- ✅ Share: `CURRENT.md` + relevant code files
- ❌ Never share: `DONE.md`, `PLAN.md`, `archive/`
- 💡 Reference files by path, don't paste entire files

---

## Model Quick Reference

```
🔴 Opus    → Phase 1, Phase 5, /pdw wrap retro, hard bugs, architecture
🟡 Sonnet  → /pdw roundtable, Phase 2, Phase 3, Phase 4 (main workhorse)
🔵 Haiku   → /pdw batch (handled automatically — developer doesn't need to know)
```

**Rule of thumb:**
> Use Opus for decisions that affect the whole product.
> Use Sonnet for everything else.
> Use `/pdw batch` for repetitive mechanical work — Haiku is handled automatically.

---

## Flexible Usage

Phases can be skipped, simplified, repeated, or combined based on task scope. You do not need to run all phases every time.

When in doubt: just type `/pdw [describe what you want]` and the Skill will guide you.
