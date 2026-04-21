# product-dev-workflow

> A branch-per-task Claude Skill for the full product & software development lifecycle.

[繁體中文](#繁體中文) | **English**

---

## Demo

<!-- DEMO_START -->
> 🎬 *Demo video coming soon — see [demo script](./demo-script.md) to try it yourself.*
<!-- DEMO_END -->

---

## What is this?

A Claude Skill (`SKILL.md`) that guides you through a structured, **branch-per-task** development workflow — from market discussion all the way to shipping.

**One branch = one piece of work.** Every task gets its own git branch and its own planning document. The skill tracks your focus, pauses cleanly when a hotfix hits, and resumes exactly where you left off.

Designed for: indie developers, product managers, and small teams who use Claude as a development partner.

---

## Features

- 🌿 **Branch-per-task** — one branch, one planning doc, one clear scope
- ⏸ **Auto pause / resume** — hotfix interrupts your focus; skill saves your resume point automatically
- 🎯 **7 commands, 4 daily verbs** — think · do · stop · see
- 🌐 **Bilingual** — replies in Chinese or English automatically
- 🤖 **Auto-detects task type** — feature / bugfix / hotfix / batch / refactor
- 👥 **Dynamic roundtable** — 31 roles, auto-selected by topic
- 🔀 **Model switch prompts** — Opus / Sonnet / Haiku at the right moment
- 🧹 **Context hygiene** — Claude only ever sees what it needs

---

## Commands

| Command | When to Use |
| --- | --- |
| `/pdw [description]` | Not sure where to start — describe what you want |
| `/pdw-init` | **One-time** — initialize this project (empty or existing) |
| `/pdw-talk [topic]` | Discuss, plan, or analyze — no development needed |
| `/pdw-dev [task]` | Write code — auto-detects new branch / resume / bug / batch |
| `/pdw-hotfix [desc]` | Emergency fix — auto-pauses your current branch |
| `/pdw-done` | Finish current work — complete / abandon / version wrap |
| `/pdw-status` | Where am I? Show focus / paused / queued |

### The Four Daily Verbs

```
think  →  /pdw-talk      Discuss without coding
do     →  /pdw-dev       Write code in a branch
stop   →  /pdw-done      Finish what you're on
see    →  /pdw-status    Where am I?

+ /pdw-hotfix   for emergencies (auto-pauses current branch)
+ /pdw          when you don't know which verb to use
+ /pdw-init     one-time setup, any project
```

---

## Workflow Overview

```
/pdw-init
    ↓
  🟢 Fresh project      → empty structure, ready to go
  🔵 Existing project   → interactive snapshot of all branches

      ↓

/pdw-talk [topic]        → Roundtable discussion (Sonnet)
/pdw-dev  [task]         → Open branch + planning doc
    ↓
    feature  →  🟢 heavy template  (design + tasks + PR draft)
    bugfix   →  🟡 medium template (symptoms + root cause + tests)
    hotfix   →  🔴 light template  (problem + fix + verify)
    batch    →  🔵 auto Haiku      (bulk rename / annotate / replace)

/pdw-hotfix              → Auto-pause focus, fix, then resume
/pdw-done                → Close branch, generate PR description, archive
/pdw-status              → Focus / Paused / Queued at a glance
```

---

## Model Guide

| Model | When |
| --- | --- |
| 🔴 Opus | New product, major version, retrospective, hard bugs, architecture |
| 🟡 Sonnet | Roundtable, planning, development (main workhorse) |
| 🔵 Haiku | `/pdw-dev` batch tasks — handled automatically |

---

## Branch File Templates

Each branch gets its own planning document in `.claude/records/branches/`:

| Type | Template Weight | Contents |
| --- | --- | --- |
| `hotfix/*` | 🔴 Light | Problem / Fix / Verification |
| `bugfix/*` | 🟡 Medium | Symptoms / Root cause / Solution / Tests |
| `feature/*` `refactor/*` | 🟢 Heavy | Background / Design / Tasks / Test plan / PR draft |

---

## Records Structure

All records stored locally — never committed to git.

```
project/
├── src/                        ← your code
├── .gitignore                  ← .claude/ is added automatically by /pdw-init
└── .claude/
    ├── skills/                 ← skill files
    └── records/
        ├── CURRENT.md          ← global state: focus / paused / queued
        ├── NOTES.md            ← lessons learned, decisions, gotchas
        ├── branches/           ← one file per active or paused branch
        │   ├── feature-login.md
        │   └── hotfix-submit-btn.md
        └── archive/
            ├── branches/       ← merged / abandoned branches
            └── v1.0-*.md       ← version snapshots
```

**Context hygiene rules:**

- ✅ Share with Claude: `CURRENT.md` + the current branch file
- ❌ Never share: `archive/`, paused branch files, `NOTES.md` (unless needed)

---

## How to Install

**Claude.ai Chat**

1. Download the `product-dev-workflow` folder
2. Zip it (folder must be at the root of the ZIP)
3. Go to Settings → Customize → Skills → Upload ZIP
4. Enable Code Execution in Settings → Capabilities

**Claude Code (Windows)**

```
Copy folder to:
C:\Users\<username>\.claude\skills\product-dev-workflow\
```

**Claude Code (Mac / Linux)**

```
cp -r product-dev-workflow ~/.claude/skills/
```

---

## How to Use

```bash
# First time on any project
/pdw-init

# Not sure where to start
/pdw

# Discuss an idea before building
/pdw-talk analyze the login UX options for mobile users

# Start a new feature
/pdw-dev build a login page with email, password and remember-me

# Emergency fix (auto-pauses your current work)
/pdw-hotfix login button not submitting on mobile Safari

# Mark current work done
/pdw-done

# Check where you are
/pdw-status

# Batch task
/pdw-dev rename all authUser references to currentUser across the codebase
```

---

## License

[MIT License](./LICENSE)

---

## 繁體中文

### 這是什麼？

一個 Claude Skill（`SKILL.md`），以「一個分支對應一個工作項目」的方式，引導你完成完整的產品開發流程。

每個任務有自己的 git branch 和規劃文件。Skill 追蹤你目前的工作焦點，遇到 hotfix 時自動暫停並記錄接回點，修完後自動提示繼續。

適合：獨立開發者、產品經理、小型團隊。

### 功能特色

- 🌿 **Branch-per-task** — 一個分支一份規劃文件，範圍清楚
- ⏸ **自動暫停 / 接回** — hotfix 打斷主線，自動儲存接回點
- 🎯 **7 個指令，4 個日常動詞** — 想 · 做 · 停 · 看
- 🌐 **自動雙語** — 你用什麼語言，Claude 就回什麼語言
- 🤖 **自動判斷任務類型** — feature / bugfix / hotfix / batch / refactor
- 👥 **動態圓桌** — 31 個角色，根據主題自動選出與會者
- 🔀 **Model 切換提示** — 對的時機提示 Opus / Sonnet / Haiku
- 🧹 **Context 整理** — Claude 永遠只看到它需要看的東西

### 指令列表

| 指令 | 使用時機 |
| --- | --- |
| `/pdw [描述]` | 不確定從哪開始，描述需求 |
| `/pdw-init` | **一次性** — 初始化專案（空的或既有的都適用） |
| `/pdw-talk [主題]` | 討論、規劃、分析，不需要開發 |
| `/pdw-dev [任務]` | 寫程式 — 自動判斷新 branch / 繼續 / bug / 批次 |
| `/pdw-hotfix [描述]` | 緊急修復 — 自動暫停當前 branch |
| `/pdw-done` | 結束當前工作 — 完成 / 放棄 / 版本封存 |
| `/pdw-status` | 我在哪？顯示 focus / paused / queued |

### 四個日常動詞

```
想  →  /pdw-talk      討論，不動程式碼
做  →  /pdw-dev       在 branch 裡寫程式
停  →  /pdw-done      結束當前工作
看  →  /pdw-status    我在哪？

+ /pdw-hotfix   緊急修復（自動暫停主線）
+ /pdw          不知道用哪個就用這個
+ /pdw-init     一次性初始化，任何專案都適用
```

### 安裝方式

**Claude.ai Chat**

1. 下載 `product-dev-workflow` 資料夾
2. 壓縮成 ZIP（資料夾要在 ZIP 根目錄）
3. 前往 Settings → Customize → Skills → 上傳 ZIP
4. 確認 Settings → Capabilities → Code Execution 已開啟

**Claude Code（Windows）**

```
複製資料夾到：
C:\Users\<你的使用者名稱>\.claude\skills\product-dev-workflow\
```

**Claude Code（Mac / Linux）**

```
cp -r product-dev-workflow ~/.claude/skills/
```

### 使用方式

```bash
# 任何專案的第一步
/pdw-init

# 開始一個新功能
/pdw-dev 建立登入頁面，包含 email、密碼欄位與記住我

# 緊急修復（自動暫停你正在做的事）
/pdw-hotfix 登入按鈕在 mobile Safari 上無法送出

# 標記完成
/pdw-done

# 查看進度
/pdw-status
```

### 授權

[MIT License](./LICENSE)
