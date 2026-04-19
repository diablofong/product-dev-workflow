# product-dev-workflow

> A streamlined Claude Skill for the full product & software development lifecycle.

[繁體中文](#繁體中文) | **English**

---

## What is this?

A Claude Skill (`SKILL.md`) that guides Claude through a structured, developer-friendly product development workflow — from market evaluation to system development.

Designed for: indie developers, product managers, and small teams who use Claude as a development partner.

## Features

- 🎯 Simple commands — just 7 to remember
- 🌐 Bilingual — replies in Chinese or English automatically
- 🤖 Auto-detects task type — just describe what you want
- 👥 Dynamic roundtable — 31 roles, auto-selected by topic
- 🔀 Model switch prompts — Opus / Sonnet / Haiku at the right moment
- 📝 Built-in records management — CURRENT / DONE / PLAN / archive
- 🧹 Context hygiene — keeps Claude sessions clean and efficient

## Commands

| Command | When to Use |
|---|---|
| `/pdw [description]` | Just describe what you want — Skill figures out the rest |
| `/pdw` | Not sure where to start — show the menu |
| `/pdw roundtable [topic]` | Standalone discussion, market analysis, no dev required |
| `/pdw dev [task]` | Start a development task |
| `/pdw bug [description]` | Fix a bug or hotfix |
| `/pdw batch [description]` | Batch repetitive tasks — rename, update, annotate, replace |
| `/pdw done [task]` | Mark task complete + update records |
| `/pdw status` | Show current progress |
| `/pdw wrap` | Archive current version + retrospective |

## Workflow Overview

```
/pdw [need]  →  Auto-detect
                    ↓
             New product/version  →  Phase 1  🔴 Opus
             Needs discussion?    →  Roundtable  🟡 Sonnet
             Ready to plan        →  Phase 3  🟢 Sonnet
             Ready to build       →  Phase 4  🟢 Sonnet
             Version complete     →  /pdw wrap  🔴 Opus

/pdw roundtable  →  Standalone discussion (independent of dev)
/pdw dev         →  Development task
/pdw bug         →  Bug fix / hotfix
/pdw batch       →  Batch repetitive tasks (Haiku handled automatically)
```

## Model Guide

| Model | When to Use |
|---|---|
| 🔴 Opus | New product, major version, retrospective, hard bugs, architecture |
| 🟡 Sonnet | Roundtable, execution planning, development (main workhorse) |
| 🔵 Haiku | `/pdw batch` — handled automatically, no need to know the model name |

The skill prompts you to switch at the right moment and waits for confirmation.

## How to Switch Models

**Claude.ai Chat**
> Click the model name at the top → Select the model

**Claude Code**
```bash
/model opus      # Switch to Opus
/model sonnet    # Switch to Sonnet
/model haiku     # Switch to Haiku
/model opusplan  # Opus for planning, auto-switch to Sonnet
```

## Records Structure

All records stored locally — never committed to git.

```
project/
├── src/                  ← your code
├── .gitignore            ← add .claude/ here
└── .claude/
    ├── skills/           ← skill files
    └── records/
        ├── CURRENT.md    ← active tasks (only this in context)
        ├── DONE.md       ← completed (never share with Claude)
        ├── PLAN.md       ← full plan (reference only)
        ├── NOTES.md      ← lessons learned, gotchas
        └── archive/      ← old versions (never in context)
```

Add to `.gitignore`:
```
.claude/
```

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
```bash
cp -r product-dev-workflow ~/.claude/skills/
```

## How to Use

```bash
# Not sure where to start
/pdw

# Describe what you want — Skill figures out the rest
/pdw I want to add a paid subscription feature

# Standalone discussion
/pdw roundtable analyze the backup software market in Taiwan

# Daily development
/pdw dev          # work on next task
/pdw bug          # fix a bug
/pdw done         # mark complete
/pdw status       # check progress
/pdw wrap         # archive this version
```

Claude replies in whichever language you write in — no setup needed.

## License

[MIT License](./LICENSE)

---

## 繁體中文

### 這是什麼？

一個 Claude Skill（`SKILL.md`），用簡單的斜線指令引導你完成完整的產品開發流程。

適合：獨立開發者、產品經理、小型團隊。

### 功能特色

- 🎯 只需記住 7 個指令
- 🌐 自動雙語 — 你用什麼語言，Claude 就回什麼語言
- 🤖 自動判斷任務類型 — 描述需求，Skill 幫你決定
- 👥 動態圓桌 — 31 個角色，根據主題自動選出與會者
- 🔀 Model 切換提示 — 對的時機提示 Opus / Sonnet / Haiku
- 📝 內建紀錄管理 — CURRENT / DONE / PLAN / archive
- 🧹 Context 整理 — 保持 Claude 對話乾淨有效率

### 指令列表

| 指令 | 使用時機 |
|---|---|
| `/pdw [描述]` | 描述你的需求，Skill 自動判斷怎麼處理 |
| `/pdw` | 不知道從哪開始，顯示選單 |
| `/pdw roundtable [主題]` | 獨立討論、市場分析，不需要開發 |
| `/pdw dev [任務]` | 開始開發任務 |
| `/pdw bug [描述]` | 修 bug 或緊急修復 |
| `/pdw batch [描述]` | 批次重複性任務 — 改名、更新、加註解、替換 |
| `/pdw done [任務]` | 標記完成 + 更新紀錄 |
| `/pdw status` | 查看當前進度 |
| `/pdw wrap` | 封存當前版本 + 回顧 |

### 流程概覽

```
/pdw [需求]  →  自動判斷
                    ↓
             新產品/版本  →  Phase 1  🔴 Opus
             需要討論？   →  圓桌     🟡 Sonnet
             準備規劃     →  Phase 3  🟢 Sonnet
             準備開發     →  Phase 4  🟢 Sonnet
             版本完成     →  /pdw wrap  🔴 Opus

/pdw roundtable  →  獨立圓桌討論（不綁開發）
/pdw dev         →  開發任務
/pdw bug         →  修 bug / 緊急修復
/pdw batch       →  批次重複性任務（自動使用最省的 Model）
```

### Model 建議

| Model | 使用時機 |
|---|---|
| 🔴 Opus | 新產品、大版本、回顧、困難 bug、架構決策 |
| 🟡 Sonnet | 圓桌、執行計畫、開發（主力）|
| 🔵 Haiku | `/pdw batch` 自動使用，開發者不需要知道 Model 名稱 |

### 如何切換 Model

**Claude.ai Chat**
> 點對話框上方的 Model 名稱 → 選擇對應 Model

**Claude Code**
```bash
/model opus      # 切換到 Opus
/model sonnet    # 切換到 Sonnet
/model haiku     # 切換到 Haiku
/model opusplan  # Opus 規劃 + 自動切回 Sonnet
```

### 紀錄檔案結構

所有紀錄存在本地，不上 git。

```
專案資料夾/
├── src/                  ← 你的程式碼
├── .gitignore            ← 加入 .claude/
└── .claude/
    ├── skills/           ← Skill 檔案
    └── records/
        ├── CURRENT.md    ← 當前任務（唯一需要給 Claude 的）
        ├── DONE.md       ← 已完成（不要給 Claude）
        ├── PLAN.md       ← 完整計畫（參考用）
        ├── NOTES.md      ← 踩坑紀錄、決策說明
        └── archive/      ← 封存舊版本（永遠不帶入 context）
```

`.gitignore` 加上：
```
.claude/
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
```bash
cp -r product-dev-workflow ~/.claude/skills/
```

### 使用方式

```bash
# 不知道從哪開始
/pdw

# 描述需求，Skill 自動判斷
/pdw 我想加一個付費訂閱功能

# 獨立圓桌討論
/pdw roundtable 分析台灣備份軟體市場

# 日常開發
/pdw dev          # 開始開發下一個任務
/pdw bug          # 修 bug
/pdw done         # 標記完成
/pdw status       # 查看進度
/pdw wrap         # 封存這個版本

# 批次重複性任務
/pdw batch 把這些函式名稱改成 camelCase：[清單]
```

Claude 會依照你輸入的語言自動回應，不需要額外設定。

### 授權

[MIT License](./LICENSE)
