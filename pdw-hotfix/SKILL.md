---
name: pdw-hotfix
description: Product Dev Workflow — emergency fix. Use when the user types /pdw-hotfix [description] to handle an urgent bug that interrupts current work.
---

# /pdw-hotfix — Emergency Interrupt

**Language rule:** Always reply in the same language the user writes in. Translate all prompts and output. Keep git commands and branch names in English.

**Model: 🟡 Sonnet** (🔴 Opus for hard bugs)

## Step 1 — Auto-Pause Current Focus

If a focus branch exists in `CURRENT.md`, record the pause point:
- Auto-detect current context if possible
- Ask user for next step in one line (in their language)

Save to branch file. Move focus → paused in `CURRENT.md`.

## Step 2 — Open Hotfix Branch

```
git checkout -b hotfix/[short-desc]
```

Create branch file using 🔴 Light template. Section labels in user's language.

## Step 3 — Assess and Fix

| Severity | Criteria | Approach |
| --- | --- | --- |
| P0 | Production down / data loss | Fix immediately, minimal docs |
| Hard | Can't identify root cause | Prompt Opus switch |
| Standard | Clear cause | Normal light-template flow |

Prompt model switch in user's language when needed.

## On Completion

When `/pdw-done` is called on this hotfix, offer to resume the paused branch. Show the saved resume point. Ask in user's language.
