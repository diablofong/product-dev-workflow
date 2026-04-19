---
name: product-dev-workflow
description: A flexible product and software development workflow skill covering the full lifecycle — from market evaluation and roundtable discussion to execution planning and system development. Use this skill whenever the user mentions: starting a new product, planning a new version, adding a feature, conducting a roundtable discussion, building an execution plan, or starting system development. Also use when the user says things like "I want to develop", "let's plan", "new feature", "new product", "roundtable", or "execution plan". Supports bilingual output (Traditional Chinese / English). Automatically determines the entry phase based on task size and type.
---

# Product Development Workflow Skill

A flexible, phase-based workflow for product and software development.
Supports: New products · New versions · Feature additions · Bug fixes
Language: Determined at session start (Traditional Chinese or English)

---

## Step 0 — Session Initialization

At the start of every session, ask:

> 🌐 **Language / 語言？**
> [1] 繁體中文
> [2] English
>
> 📋 **What would you like to do? / 你想做什麼？**
> [A] 新產品 / New Product → Phase 1
> [B] 新版本 / New Version → Phase 1
> [C] 大功能 / Major Feature → Phase 1 or 2
> [D] 新功能 / New Feature → Phase 4
> [E] 圓桌討論 / Roundtable → Phase 2
> [F] Bug 修復 / Bug Fix → Quick Fix Track
> [G] 優化 / Optimization → Quick Fix Track

Once language and entry point are selected, proceed accordingly. All subsequent output must be in the selected language.

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

### Work Items
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
- If a blocker is identified, suggest switching to Opus

---

## Phase 5 — Next Version Planning
**Model: 🔴 Opus**
**Trigger: Current version is complete / Ready to plan next cycle**

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

## Model Quick Reference

```
🔴 Opus    → Phase 1, Phase 5, hard bugs, architecture decisions
🟡 Sonnet  → Phase 2, Phase 3, Phase 4 (main workhorse)
🔵 Haiku   → Boilerplate, renaming, log lines, simple lookups
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

## Trigger Keywords (for reference)

The skill activates when the user mentions:

**Chinese:** 新產品、新版本、大功能、新功能、圓桌、建立計畫、開始開發、規劃、市場評估、系統開發、Bug修復、優化

**English:** new product, new version, major feature, new feature, roundtable, execution plan, start development, planning, market evaluation, system development, bug fix, optimization
