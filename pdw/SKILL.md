---
name: pdw
description: Product Dev Workflow — universal entry point. Use when the user types /pdw with or without a description.
---

# /pdw — Universal Router

**Language rule:** Always reply in the same language the user writes in. Chinese input → Chinese output. English input → English output. Never mix unless the user does.

## Pre-check

Before routing, check if `.claude/records/` exists:
- Not found → suggest `/pdw-init` first (don't block `/pdw-talk`)
- Found → proceed

## Routing Rules

| Description signals | Route to |
| --- | --- |
| Discussion / analysis / ideation / no dev needed | `/pdw-talk` |
| Urgent / P0 / hotfix / production down | `/pdw-hotfix` |
| Feature / bug / refactor / batch / build | `/pdw-dev` |
| No description | Show menu |

## Menu (no description)

Translate all menu text to match the user's language.

```
📋 Product Dev Workflow

  /pdw [description]   Universal — describe what you want
  /pdw-init            Initialize this project (one-time)
  /pdw-talk            Roundtable discussion (no dev)
  /pdw-dev             Development (branch / resume / bug / batch)
  /pdw-hotfix          Emergency fix (auto-pauses current branch)
  /pdw-done            Finish current work
  /pdw-status          Show current state

Not sure? Just type: /pdw [describe your need]
```
