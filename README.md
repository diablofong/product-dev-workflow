# product-dev-workflow

> A branch-per-task Claude Skill for the full product & software development lifecycle. **One slash command, seven sub-flows.**

[繁體中文](#繁體中文) | **English**

---

<!-- DEMO_START -->
> 🎬 *Demo video* 
<!-- DEMO_END -->
https://github.com/user-attachments/assets/f4402f45-f5a2-49e1-bf81-acea583b52cd

---

## What is this?

A Claude Skill (`SKILL.md`) that gives you a single `/pdw` command covering the entire product development lifecycle — discussion, development, hotfix handling, completion, status tracking.

**Everything lives under one command.** You type `/pdw [sub-command] [args]` or just `/pdw [natural language]` and the skill picks the right flow. No need to memorize seven separate commands.

**Branch-per-task.** Every task gets its own git branch + its own planning document. The skill tracks focus, auto-pauses for hotfixes, and resumes exactly where you left off.

Designed for: indie developers, product managers, and small teams who use Claude as a development partner.

---

## Features

- 🎯 **One slash command, seven sub-flows** — `/pdw init`, `talk`, `dev`, `hotfix`, `done`, `status` + menu
- 🧠 **Natural language routing** — `/pdw I want to add a login page` auto-routes to the right sub-flow
- 🌿 **Branch-per-task** — one branch, one planning doc, one clear scope
- ⏸ **Auto pause / resume** — hotfix interrupts your focus; skill saves the resume point automatically
- 🌐 **Auto bilingual** — write in Chinese, get Chinese; write in English, get English
- 👥 **Dynamic roundtable** — 31 roles, auto-selected by topic
- 🔀 **Model switch prompts** — Opus / Sonnet / Haiku at the right moment
- 🧹 **Context hygiene** — Claude only ever sees what it needs

---

## Sub-Commands

| Command | When to Use |
| --- | --- |
| `/pdw` | Show the menu |
| `/pdw init` | **One-time** — initialize this project (empty or existing) |
| `/pdw talk <topic>` | Discuss, plan, analyze — no development needed |
| `/pdw dev <task>` | Write code — auto-detects new branch / resume / bug / batch |
| `/pdw hotfix <desc>` | Emergency — auto-pauses your current branch |
| `/pdw done [abandon]` | Finish current work — complete / abandon / version wrap |
| `/pdw status` | Where am I? Focus / paused / queued |

### Or just describe what you want

```
/pdw I want to build a subscription feature
/pdw analyze the Taiwan backup software market
/pdw payment API is 500ing in production right now
```

The skill reads your intent and routes to the right sub-flow automatically.

---

## Workflow Overview

```
/pdw init
    ↓
  🟢 Fresh project      → empty structure, ready to go
  🔵 Existing project   → interactive snapshot of all branches

      ↓

/pdw talk <topic>        → Roundtable discussion (Sonnet)
/pdw dev  <task>         → Open branch + planning doc
    ↓
    feature  →  🟢 heavy template  (design + tasks + PR draft)
    bugfix   →  🟡 medium template (symptoms + root cause + tests)
    hotfix   →  🔴 light template  (problem + fix + verify)
    batch    →  🔵 auto Haiku      (bulk rename / annotate / replace)

/pdw hotfix              → Auto-pause focus, fix, then resume
/pdw done                → Close branch, generate PR description, archive
/pdw status              → Focus / Paused / Queued at a glance
```

---

## Model Guide

| Model | When |
| --- | --- |
| 🔴 Opus | New product, major version, retrospective, hard bugs, architecture |
| 🟡 Sonnet | Roundtable, planning, development (main workhorse) |
| 🔵 Haiku | Batch tasks — handled automatically via `/pdw dev` |

---

## Branch Templates

Each branch gets its own planning document in `.claude/records/branches/`:

| Type | Template | Contents |
| --- | --- | --- |
| `hotfix/*` | 🔴 Light | Problem / Fix / Verification |
| `bugfix/*` | 🟡 Medium | Symptoms / Root cause / Solution / Tests |
| `feature/*` · `refactor/*` | 🟢 Heavy | Background / Design / Tasks / Test plan / PR draft |

---

## Records Structure

All records stored locally — never committed to git.

```
project/
├── src/                        ← your code
├── .gitignore                  ← .claude/ added automatically by /pdw init
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

**Claude Code (Mac / Linux)**

```bash
# Extract the zip directly into your skills folder
unzip product-dev-workflow.zip -d ~/.claude/skills/
```

After extraction, your skills folder should look like:

```
~/.claude/skills/
└── pdw/
    └── SKILL.md
```

**Claude Code (Windows)**

```powershell
Expand-Archive product-dev-workflow.zip -DestinationPath $HOME\.claude\skills\
```

**Claude.ai Chat**

1. Zip the `pdw/` folder (the folder itself should be at the root of the ZIP)
2. Go to Settings → Customize → Skills → Upload ZIP
3. Enable Code Execution in Settings → Capabilities

**Restart Claude Code after installing**, then type `/pdw` to see the menu.

---

## How to Use

```bash
# First time on any project
/pdw init

# Discuss an idea before building
/pdw talk analyze the login UX options for mobile users

# Start a new feature
/pdw dev build a login page with email, password and remember-me

# Emergency fix (auto-pauses your current work)
/pdw hotfix login button not submitting on mobile Safari

# Mark current work done
/pdw done

# Abandon current branch
/pdw done abandon

# Check where you are
/pdw status

# Not sure? Just describe it
/pdw I want to refactor the authentication layer
```

---

## License

[MIT License](./LICENSE)

---

## 繁體中文

### 這是什麼？

一個 Claude Skill，用**單一 `/pdw` 指令**涵蓋完整的產品開發流程——討論、開發、緊急修復、完成、狀態追蹤。

**全部功能都在一個指令底下。** 你打 `/pdw [子指令] [參數]` 或直接 `/pdw [自然語言]`，Skill 會自動選擇正確的流程，不需要記七個分開的指令。

**Branch-per-task。** 每個任務有自己的 git branch 和規劃文件。Skill 追蹤工作焦點，遇到 hotfix 自動暫停並記錄接回點，修完後自動提示繼續。

適合：獨立開發者、產品經理、小型團隊。

### 功能特色

- 🎯 **單一指令，七個子流程** — `/pdw init`、`talk`、`dev`、`hotfix`、`done`、`status` + 選單
- 🧠 **自然語言路由** — `/pdw 我想加登入功能`，Skill 自動選對流程
- 🌿 **Branch-per-task** — 一個分支一份規劃文件
- ⏸ **自動暫停 / 接回** — hotfix 打斷主線，自動儲存接回點
- 🌐 **自動雙語** — 中文輸入全中文回覆，英文輸入全英文回覆
- 👥 **動態圓桌** — 31 個角色，根據主題自動選出與會者
- 🔀 **Model 切換提示** — 對的時機提示 Opus / Sonnet / Haiku
- 🧹 **Context 整理** — Claude 永遠只看到它需要看的東西

### 子指令列表

| 指令 | 使用時機 |
| --- | --- |
| `/pdw` | 顯示選單 |
| `/pdw init` | **一次性** — 初始化專案（空的或既有的都適用） |
| `/pdw talk <主題>` | 討論、規劃、分析，不需要開發 |
| `/pdw dev <任務>` | 寫程式 — 自動判斷新 branch / 繼續 / bug / 批次 |
| `/pdw hotfix <描述>` | 緊急修復 — 自動暫停當前 branch |
| `/pdw done [abandon]` | 結束當前工作 — 完成 / 放棄 / 版本封存 |
| `/pdw status` | 我在哪？顯示 focus / paused / queued |

### 或直接用自然語言描述

```
/pdw 我想做一個付費訂閱功能
/pdw 分析台灣備份軟體市場
/pdw 付款 API 在線上環境噴 500
```

Skill 會讀你的意圖，自動選對流程。

### 安裝方式

**Claude Code（Mac / Linux）**

```bash
unzip product-dev-workflow.zip -d ~/.claude/skills/
```

**Claude Code（Windows）**

```powershell
Expand-Archive product-dev-workflow.zip -DestinationPath $HOME\.claude\skills\
```

解壓後結構應該是：

```
~/.claude/skills/
└── pdw/
    └── SKILL.md
```

**重開 Claude Code 後**，輸入 `/pdw` 就會看到選單。

**Claude.ai Chat**

1. 壓縮 `pdw/` 資料夾（資料夾要在 ZIP 根目錄）
2. 前往 Settings → Customize → Skills → 上傳 ZIP
3. 確認 Settings → Capabilities → Code Execution 已開啟

### 使用方式

```bash
# 任何專案的第一步
/pdw init

# 開始一個新功能
/pdw dev 建立登入頁面，包含 email、密碼與記住我

# 緊急修復（自動暫停你正在做的事）
/pdw hotfix 登入按鈕在 mobile Safari 上無法送出

# 標記完成
/pdw done

# 放棄當前 branch
/pdw done abandon

# 查看進度
/pdw status

# 不確定就直接描述
/pdw 我想重構認證模組
```

### 授權

[MIT License](./LICENSE)
