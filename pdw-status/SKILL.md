---
name: pdw-status
description: Product Dev Workflow — show current state. Use when the user types /pdw-status to see the active branch, paused branches, and queued work.
---

# /pdw-status — Show Current State

**Language rule:** Always reply in the same language the user writes in. Translate all labels and messages. Keep branch names and file paths in English.

Read `CURRENT.md` and display in user's language:

```
📊 [Current State / 目前狀態] — [datetime]
────────────────────────────────

🎯 [Focus / 焦點]
   [branch-name]   (started X days ago)
   Next: [next step]
   Tasks: X of Y done

⏸ [Paused / 暫停中] (N)
   [branch-name]
     paused X days ago
     next: "[resume instruction]"

🔜 [Queued / 排隊中] (N)
   [branch-name] — [note]

────────────────────────────────
Version: vX.x · N branches merged
```

If `.claude/records/` not found: tell user to run `/pdw-init` first (in their language).

Context hygiene reminder (show occasionally, in user's language):
- Share with Claude: `CURRENT.md` + active branch file only
- Never share: `archive/`, paused branch files
