---
name: product-dev-workflow
description: "A flexible product and software development workflow skill. Activated ONLY via slash commands (/pdw, /pdw-new, /pdw-version, /pdw-feature, /pdw-roundtable, /pdw-plan, /pdw-dev, /pdw-bug). Do NOT auto-trigger from conversation keywords, file contents, or any other context. Only load this skill when the user explicitly types one of the above slash commands."
---

# Product Development Workflow Skill

A flexible, phase-based workflow for product and software development.
Supports: New products · New versions · Feature additions · Bug fixes
Language: Determined at session start (Traditional Chinese or English)

---

## Slash Commands

Use these commands to jump directly to any phase:

| Command | Action | Model |
|---|---|---|
| `/pdw` | Show this menu | — |
| `/pdw-new` | New product → Phase 1 | 🔴 Opus |
| `/pdw-version` | New version → Phase 1 | 🔴 Opus |
| `/pdw-feature` | New feature → auto-detect phase | 🟡 depends |
| `/pdw-roundtable` | Roundtable discussion → Phase 2 | 🟡 Sonnet |
| `/pdw-plan` | Execution planning → Phase 3 | 🟢 Sonnet |
| `/pdw-dev` | System development → Phase 4 | 🟢 Sonnet |
| `/pdw-bug` | Bug fix / Optimization → Quick Fix | 🟢 Sonnet |
| `/pdw-haiku` | Batch mechanical tasks | 🔵 Haiku |

---

## Step 0 — Session Initialization

When `/pdw` is called with no arguments, display:

```
📋 Product Dev Workflow — Command Menu

  /pdw-new          New product        → Phase 1   🔴 Opus
  /pdw-version      New version        → Phase 1   🔴 Opus
  /pdw-feature      New feature        → Auto       🟡
  /pdw-roundtable   Roundtable         → Phase 2   🟡 Sonnet
  /pdw-plan         Execution plan     → Phase 3   🟢 Sonnet
  /pdw-dev          Development        → Phase 4   🟢 Sonnet
  /pdw-bug          Bug fix / Optimize → Quick Fix  🟢 Sonnet
  /pdw-haiku        Batch tasks        → Haiku      🔵 Haiku

Language: reply in the same language the user writes in.
```

When a specific command is called, skip the menu and go directly to the correct phase. Ask for the product/task description if not already provided.

---

## Entry Point Decision Logic

Before starting, determine the correct entry phase by asking three questions:

```
Q1: Will this task affect the product direction or business model?
    Yes → Start at Phase 1 (use Opus)

Q2: Does this require cross-team coordination or multiple roles?
    Yes → Start at Phase 2 (use Sonnet)

Q3: Is this a straightforward development task?
    Yes → Start at Phase 4 (use Sonnet)
```

### Quick Reference Table

| Scenario | Entry Phase | Model |
|---|---|---|
| Brand new product | Phase 1 | 🔴 Opus |
| Major version (v2.0+) | Phase 1 | 🔴 Opus |
| Feature affecting architecture | Phase 1 or 2 | 🔴 Opus / 🟡 Sonnet |
| Medium feature (cross-team) | Phase 2 | 🟡 Sonnet |
| Small feature (single dev) | Phase 4 | 🟢 Sonnet |
| Bug fix | Quick Fix Track | 🟢 Sonnet |

---

## Phase 1 — Product Planning & Market Evaluation
**Model: 🔴 Opus**
**Trigger: New product / New major version / Large feature affecting direction**

### ⚠️ Before Starting Phase 1 — Switch Model

Display this message and WAIT for user confirmation before proceeding:

---
🔴 **Phase 1 requires Opus**

Please switch your model now:
- **Claude.ai Chat** → Click the model name at the top of the chat → Select Opus
- **Claude Code** → Type `/model opus` in the terminal

Type **"continue"** when ready.
- Define core pain point and user problem
- Identify target users
- Competitive analysis (existing alternatives, pricing, gaps)
- Differentiation strategy
- Business model (free / freemium / paid / open source)
- Initial roadmap and priorities
- Technical feasibility overview

### Auto Output After Phase 1

```
📄 PRODUCT BRIEF
────────────────────────────────
Product Name     :
Core Pain Point  :
Target Users     :
Competitors      :
Differentiation  :
Business Model   :
Initial Roadmap  :
Open Questions   :
────────────────────────────────
```

> ➡️ Proceed to Phase 2 (Roundtable Discussion)? [Yes / No / Skip to Phase 3]

---

## Phase 2 — Roundtable Discussion
**Model: 🟡 Sonnet**
**Trigger: Multi-role coordination needed / After Phase 1**

### ⚠️ Before Starting Phase 2 — Switch Model

Display this message and WAIT for user confirmation before proceeding:

---
🟡 **Phase 2 requires Sonnet**

Please switch your model now:
- **Claude.ai Chat** → Click the model name → Select Sonnet
- **Claude Code** → Type `/model sonnet` in the terminal

Type **"continue"** when ready.

---

Do not proceed until the user confirms.

### Role Selection
Ask the user which roles to include (select all that apply):

- 🏗 Technical Architect
- 📋 Product Manager
- 🎨 UX Designer
- 📣 Marketing
- ⚖️ Legal / Privacy
- 🧪 QA Engineer
- 📊 Business Analyst

Each selected role provides their perspective on the topic. Roles should challenge each other where appropriate. Conclude with a consensus summary and list of unresolved items.

### Auto Output After Phase 2

```
📄 ROUNDTABLE SUMMARY
────────────────────────────────
Topic            :
Date             :

[Role] Perspective:
  ...

[Role] Perspective:
  ...

Consensus        :
Action Items     :
Open Questions   :
────────────────────────────────
```

> ➡️ Proceed to Phase 3 (Execution Planning)? [Yes / No / Skip to Phase 4]

---

## Phase 3 — Execution Planning (Per Department)
**Model: 🟢 Sonnet**
**Trigger: After roundtable / When direction is clear and needs to be broken down**

### Document Selection
Ask the user which documents to produce (select all that apply):

- [ ] PRD — Product Requirements Document
- [ ] Technical Specification
- [ ] Roadmap
- [ ] UI/UX Flow
- [ ] Test Plan
- [ ] Marketing Plan

Produce each selected document in sequence. Each document should be complete enough to hand off to the relevant team.

### Auto Output After Phase 3

```
📄 EXECUTION PLAN SUMMARY
────────────────────────────────
Documents Produced :
Key Milestones     :
Dependencies       :
Risks              :
────────────────────────────────
```

> ➡️ Proceed to Phase 4 (Development)? [Yes / No]

---

## Phase 4 — System Development
**Model: 🟢 Sonnet (switch to 🔴 Opus when stuck)**
**Trigger: Ready to build / After planning phases**

### Development Task Routing

| Task Type | Model | Notes |
|---|---|---|
| Feature implementation | 🟢 Sonnet | Default |
| Writing tests | 🟢 Sonnet | |
| Refactoring | 🟢 Sonnet | |
| Known bug fix | 🟢 Sonnet | |
| Hard / mysterious bug | 🔴 Opus | Switch when stuck |
| Cross-module refactor | 🔴 Opus | |
| Architecture decision | 🔴 Opus | |
| Boilerplate / rename / logs | 🔵 Haiku | Most economical |

### During Development
- Follow any project-specific coding conventions if provided
- After each task, ask: "Next task, or is there a blocker?"
- If a blocker is identified, display this and wait for confirmation:

---
🔴 **Blocker detected — Consider switching to Opus**

- **Claude.ai Chat** → Click the model name → Select Opus
- **Claude Code** → Type `/model opus` in the terminal

Type **"continue"** when ready, or **"skip"** to stay on Sonnet.

---

---

## Phase 5 — Next Version Planning
**Model: 🔴 Opus**
**Trigger: Current version is complete / Ready to plan next cycle**

### ⚠️ Before Starting Phase 5 — Switch Model

Display this message and WAIT for user confirmation before proceeding:

---
🔴 **Phase 5 requires Opus**

Please switch your model now:
- **Claude.ai Chat** → Click the model name → Select Opus
- **Claude Code** → Type `/model opus` in the terminal

Type **"continue"** when ready.

---

Do not proceed until the user confirms.

Return to Phase 1 with the current product as context. Carry forward:
- What worked well
- What didn't
- User feedback
- Technical debt to address

---

## Quick Fix Track (Bug Fix / Optimization)
**Model: 🟢 Sonnet (switch to 🔴 Opus for hard bugs)**
**Trigger: Bug fix / Performance optimization / Does NOT go through Phase flow**

### Bug Fix Flow
1. Understand the bug (symptoms, when it occurs, expected vs actual)
2. Hypothesize root cause
3. Propose fix with code
4. Explain how to verify the fix
5. Suggest a test to prevent regression

### Optimization Flow
1. Understand the bottleneck
2. Profile or identify the slow path
3. Propose optimization
4. Explain tradeoffs

---

## /pdw-haiku — Batch Mechanical Tasks
**Model: 🔵 Haiku**
**Trigger: /pdw-haiku [task description] [list or details]**

### Suitable Tasks (mechanical, no logic needed)
- Execute a method against a list of items
- Batch rename (variables, functions, files)
- Batch add comments / annotations
- Batch format or reformat code
- Batch string replacement
- Generate boilerplate from a fixed pattern
- Any "give me a list, run the same action" task

### Usage Format
```
/pdw-haiku [task description] [list or details]
```

### Examples
```
/pdw-haiku call updateStatus() on this list: id:001→active, id:002→inactive
/pdw-haiku add error handling log to these functions: [list]
/pdw-haiku rename these variables to camelCase: [list]
/pdw-haiku call sendNotification() for each id in this list: [list]
```

### Auto-check Before Executing
After receiving the task, evaluate whether it is suitable for Haiku:

✅ Proceed with Haiku:
- Same action repeated across items
- No need to interpret the meaning of each item
- Pure mechanical operation

❌ Switch to Sonnet and notify user:
- Each item requires individual judgment
- Involves business logic or rules
- Requires understanding of context

### ⚠️ Model Switch Prompt

---
🔵 **This task is suitable for Haiku**

Please switch your model now:
- **Claude Code** → `/model haiku`
- **Claude.ai Chat** → Click the model name → Select Haiku

Type **"continue"** when ready.

---

Do not proceed until the user confirms.

### Execution & Report
After execution, report results in the clearest format for the task type. Examples:

```
Batch method execution:
✅ sendNotification(001) → success
✅ sendNotification(002) → success
❌ sendNotification(003) → failed (reason)

Batch rename:
✅ getUserInfo() → fetchUserProfile() (3 files)
✅ setData() → updateData() (1 file)

Batch add comments:
✅ calculateTax() → comment added
✅ formatDate() → comment added
```

Always end with a summary:
```
Total: X items — Success: X, Failed: X
Failed reason: [if any]
```

After completion, prompt the user to switch back:

---
🟢 **Batch complete — switch back to Sonnet**

- **Claude Code** → `/model sonnet`
- **Claude.ai Chat** → Click the model name → Select Sonnet

---

---

## Model Quick Reference

```
🔴 Opus    → Phase 1, Phase 5, hard bugs, architecture decisions
🟡 Sonnet  → Phase 2, Phase 3, Phase 4 (main workhorse)
🔵 Haiku   → /pdw-haiku, batch mechanical tasks, no logic needed
```

**Rule of thumb:**
> Use Opus when making difficult decisions that affect the whole product.
> Use Sonnet for everything else.
> Switch to Opus mid-phase only when genuinely stuck.

---

## Flexible Phase Usage

Phases can be:
- **Skipped** — if not relevant to the task
- **Simplified** — do only the parts that matter
- **Repeated** — if new information changes the direction
- **Combined** — e.g., Phase 2 + 3 in one session for smaller features

You do not need to run all 5 phases for every task. Match the depth to the scope of the work.

---

## Command Reference (Quick Copy)

```bash
/pdw               # Show command menu
/pdw-new           # New product
/pdw-version       # New version
/pdw-feature       # New feature (auto-detects phase)
/pdw-roundtable    # Roundtable discussion
/pdw-plan          # Execution planning
/pdw-dev           # System development
/pdw-bug           # Bug fix or optimization
/pdw-haiku         # Batch mechanical tasks (no logic needed)
```

**Language:** Claude replies in the same language you write in — Chinese or English, no setup needed.
