---
name: pdw-init
description: Product Dev Workflow — one-time project initialization. Use when the user types /pdw-init to set up the workflow on a new or existing project.
---

# /pdw-init — Initialize Project (One-Time)

**Language rule:** Always reply in the same language the user writes in. Translate all output text, prompts, and template placeholder labels to match. The only exception: git commands and file paths stay in English.

**Model: 🟡 Sonnet**

## Pre-check

If `.claude/records/` already exists → abort and tell the user to run `/pdw-status` instead.

## Step 1 — Detect Project Type

Ask user to run (or run automatically if tools allow):
```
git branch --list
git branch --show-current
git log --oneline -1 2>/dev/null
git status --short
```

| Condition | Mode |
| --- | --- |
| No commits / no branches beyond main | 🟢 Fresh Mode |
| Has non-main branches or uncommitted work | 🔵 Onboard Mode |

If ambiguous, ask the user to pick between Fresh and Onboard.

---

## 🟢 Fresh Mode

Create structure, no questions:

```
.claude/records/
├── CURRENT.md
├── NOTES.md
├── branches/
└── archive/branches/
```

CURRENT.md template (labels in user's language):
```markdown
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

Offer to add `.claude/` to `.gitignore`. Output a success message in the user's language.

---

## 🔵 Onboard Mode

For each non-main branch, ask the user (in their language):
- Branch type: feature / bugfix / hotfix / refactor / unsure
- One-line description of what this branch does
- Current progress: planning / design done / mid-implementation / nearly done
- Next action (one line — the resume point)

If user can't remember a branch: offer to skip and mark it as "needs-review".

**Classify into CURRENT.md:**
- Current git branch → Focus
- Recent commits ≤7 days → Paused
- Stale >7 days → Queued (mark as stale)

Show proposed state and ask for confirmation before writing files.

**Branch file (Snapshot Mode):**
Generate from the heavy/medium/light template appropriate to the branch type. For unknown sections, write a short placeholder note in the user's language indicating it was not backfilled during onboarding.

**Rules:**
- Read-only git queries only. Never run destructive git commands.
- Idempotent: second run aborts cleanly.
- Never force retroactive documentation.
