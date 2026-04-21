---
name: pdw-dev
description: Product Dev Workflow — development. Use when the user types /pdw-dev [task] to write code, continue a branch, fix a bug, or run batch tasks.
---

# /pdw-dev — Development

**Language rule:** Always reply in the same language the user writes in. Translate all prompts, status messages, and confirmations. Keep git commands, branch names, and file paths in English.

**Model: 🟡 Sonnet** (🔴 Opus when stuck · 🔵 Haiku for batch)

## Decision Tree

```
/pdw-dev [task?]
    ↓
Focus branch in CURRENT.md?
  YES + no new task  →  RESUME
  YES + new task     →  ASK (pause current or add to current?)
  NO                 →  NEW BRANCH
```

## Branch Classification

| Signals | Type | Template | Model |
| --- | --- | --- | --- |
| batch / rename / list of ≥3 items | 🔵 batch | light | Haiku |
| bug / error / crash | 🟡 bugfix | medium | Sonnet |
| refactor / cross-module | 🟢 refactor | heavy | Sonnet/Opus |
| feature / add / build | 🟢 feature | heavy | Sonnet |

Show confirmation (in user's language) before creating branch.

## Branch Templates

All template section headers should be translated to the user's language when generating the file. Branch names stay in English.

### 🔴 Light (hotfix / simple bug)
```
Type · Created · Base

Problem / 問題:
Fix / 修法:
Verification / 驗證:
Tasks: [ ] Fix · [ ] Tested · [ ] Ready
Pause Point / 接回點:
  next step :
```

### 🟡 Medium (bugfix)
```
Type · Created · Base

Symptoms / 症狀:
Root Cause / 根因:
Solution / 方案:
Tasks: [ ] Fix · [ ] Test · [ ] Docs
Test Plan:
PR Description (draft):
Pause Point:
  next step :
```

### 🟢 Heavy (feature / refactor)
```
Type · Created · Base

Background & Goal / 背景與目標:
Design / 技術設計:
  Schema:
  API:
  Flow:
Task Breakdown / 任務清單:
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

## Resuming

Show resume state in user's language: branch name, last pause point, next step, task progress.

## Batch Mode

When batch detected, prompt model switch to Haiku. Show execution plan and confirm before running. After completion, prompt switch back to Sonnet.

## Blocker

When stuck, prompt Opus switch in user's language.

## During Development

If user mentions a lesson learned: offer to save to `NOTES.md` (ask in user's language).
