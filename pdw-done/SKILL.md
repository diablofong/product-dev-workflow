---
name: pdw-done
description: Product Dev Workflow — finish current work. Use when the user types /pdw-done to complete a branch, abandon it, or wrap a version.
---

# /pdw-done — Finish Current Work

**Language rule:** Always reply in the same language the user writes in. Translate all prompts, confirmations, and output. Keep git commands and file paths in English.

## Decision Tree

```
/pdw-done [argument?]
    ↓
"abandon"?              →  ABANDON
Focus is hotfix?        →  CLOSE HOTFIX + offer resume
All tasks done?         →  CLOSE BRANCH
Tasks remaining?        →  Ask which task, or close whole branch
No focus + all done?    →  Offer VERSION WRAP
No focus?               →  Suggest /pdw-dev
```

## Close Branch (Normal)

Generate PR description from branch file (in user's language). Confirm before archiving. Move branch file to `archive/branches/`. Clear focus in `CURRENT.md`.

## Close Hotfix

Same as normal close, then offer to resume the paused focus branch. Show saved resume point. Ask in user's language.

## Abandon

Ask for optional reason (in user's language). Archive with abandoned status.

## Version Wrap (auto-offered when last branch closes)

Prompt Opus switch for retrospective. Run retrospective (all output in user's language). Archive CURRENT.md snapshot. Reset to empty template. Ask about next version.
