# product-dev-workflow

> A flexible Claude Skill for the full product & software development lifecycle.

[繁體中文](#繁體中文) | **English**

---

## What is this?

A Claude Skill (`SKILL.md`) that guides Claude through a structured, flexible product development workflow — from market evaluation to system development.

Designed for: indie developers, product managers, and small teams who use Claude as a development partner.

## Features

- 🎯 Slash command driven — `/pdw`, `/pdw-new`, `/pdw-bug`, and more
- 🌐 Bilingual — replies in Chinese or English automatically based on what you write
- 🔀 Flexible entry points — jump to any phase directly
- 🤖 Model switch prompts — tells you when to switch Opus / Sonnet / Haiku and waits for confirmation before continuing
- 📄 Auto document output — Product Brief, Roundtable Summary, PRD, Technical Spec, and more

## Commands

| Command | Action | Model |
|---|---|---|
| `/pdw` | Show command menu | — |
| `/pdw-new` | New product → Phase 1 | 🔴 Opus |
| `/pdw-version` | New version → Phase 1 | 🔴 Opus |
| `/pdw-feature` | New feature → auto-detect phase | 🟡 depends |
| `/pdw-roundtable` | Roundtable discussion → Phase 2 | 🟡 Sonnet |
| `/pdw-plan` | Execution planning → Phase 3 | 🟢 Sonnet |
| `/pdw-dev` | System development → Phase 4 | 🟢 Sonnet |
| `/pdw-bug` | Bug fix / Optimization | 🟢 Sonnet |
| `/pdw-haiku` | Batch mechanical tasks | 🔵 Haiku |

## Workflow Overview

```
Phase 1 │ Product Planning & Market Evaluation   🔴 Opus
Phase 2 │ Roundtable Discussion (multi-role)     🟡 Sonnet
Phase 3 │ Execution Planning (per department)    🟢 Sonnet
Phase 4 │ System Development                     🟢 Sonnet
Phase 5 │ Next Version Planning                  🔴 Opus

Quick Fix Track │ Bug Fix / Optimization         🟢 Sonnet
Haiku Track     │ Batch Mechanical Tasks         🔵 Haiku
```

## When to Start at Each Phase

| Scenario | Command | Entry Phase |
|---|---|---|
| Brand new product | `/pdw-new` | Phase 1 |
| Major version (v2.0+) | `/pdw-version` | Phase 1 |
| New feature | `/pdw-feature` | Auto-detected |
| Roundtable only | `/pdw-roundtable` | Phase 2 |
| Ready to build | `/pdw-dev` | Phase 4 |
| Bug fix / Optimization | `/pdw-bug` | Quick Fix |
| Batch mechanical tasks | `/pdw-haiku` | Haiku Track |

## Model Guide

| Model | When to Use |
|---|---|
| 🔴 Opus | Phase 1 & 5, hard decisions, architecture, mysterious bugs |
| 🟡 Sonnet | Phase 2, 3, 4 — the main workhorse |
| 🔵 Haiku | `/pdw-haiku` — batch mechanical tasks, no logic needed |

The skill prompts you to switch models at the right moment and waits for your confirmation before continuing.

## How to Switch Models

**Claude.ai Chat**
> Click the model name at the top of the chat → Select the model

**Claude Code**
```bash
/model opus      # Switch to Opus
/model sonnet    # Switch to Sonnet
/model haiku     # Switch to Haiku
/model opusplan  # Opus for planning, auto-switch to Sonnet for execution
```

## How to Install

**Claude.ai Chat**
1. Download the `product-dev-workflow` folder
2. Zip it (folder must be at the root of the ZIP)
3. Go to Settings → Customize → Skills → Upload ZIP
4. Enable Code Execution in Settings → Capabilities

**Claude Code (Windows)**
```
Copy the folder to:
C:\Users\<your-username>\.claude\skills\product-dev-workflow\
```

**Claude Code (Mac / Linux)**
```bash
cp -r product-dev-workflow ~/.claude/skills/
```

## How to Use

```bash
/pdw              # Show command menu
/pdw-new          # Start planning a new product
/pdw-bug          # Fix a bug
```

Claude replies in whichever language you write in — no language setup needed.

## License

[MIT License](./LICENSE)

---

## 繁體中文

### 這是什麼？

一個 Claude Skill（`SKILL.md`），透過斜線指令引導你完成完整的產品開發流程，從市場評估到系統開發全覆蓋。

適合：獨立開發者、產品經理、小型團隊。

### 功能特色

- 🎯 斜線指令驅動 — `/pdw`、`/pdw-new`、`/pdw-bug` 等
- 🌐 自動雙語 — 你用中文說話就回中文，用英文就回英文
- 🔀 彈性入場點 — 直接跳到任意 Phase
- 🤖 Model 切換提示 — 每個關鍵點提示切換 Opus / Sonnet / Haiku，並等待確認後再繼續
- 📄 自動產出文件 — Product Brief、圓桌會議結論、PRD、技術規格書等

### 指令列表

| 指令 | 功能 | Model |
|---|---|---|
| `/pdw` | 顯示指令選單 | — |
| `/pdw-new` | 新產品 → Phase 1 | 🔴 Opus |
| `/pdw-version` | 新版本 → Phase 1 | 🔴 Opus |
| `/pdw-feature` | 新功能 → 自動判斷入場點 | 🟡 依情況 |
| `/pdw-roundtable` | 圓桌討論 → Phase 2 | 🟡 Sonnet |
| `/pdw-plan` | 建立執行計畫 → Phase 3 | 🟢 Sonnet |
| `/pdw-dev` | 系統開發 → Phase 4 | 🟢 Sonnet |
| `/pdw-bug` | Bug 修復 / 優化 | 🟢 Sonnet |
| `/pdw-haiku` | 批次機械性任務 | 🔵 Haiku |

### 流程概覽

```
Phase 1 │ 產品規劃 & 市場評估     🔴 Opus
Phase 2 │ 圓桌會議（多角色）      🟡 Sonnet
Phase 3 │ 各部門建立執行計畫      🟢 Sonnet
Phase 4 │ 系統開發                🟢 Sonnet
Phase 5 │ 下一版規劃              🔴 Opus

快速通道 │ Bug 修復 / 優化        🟢 Sonnet
批次通道 │ 機械性批次任務         🔵 Haiku
```

### 如何切換 Model

**Claude.ai Chat**
> 點對話框上方的 Model 名稱 → 選擇對應 Model

**Claude Code**
```bash
/model opus      # 切換到 Opus
/model sonnet    # 切換到 Sonnet
/model haiku     # 切換到 Haiku
/model opusplan  # Opus 規劃 + 自動切回 Sonnet 執行
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
/pdw              # 顯示指令選單
/pdw-new          # 開始規劃新產品
/pdw-bug          # 修復 Bug
```

Claude 會依照你輸入的語言自動回應，不需要額外設定。

### 授權

[MIT License](./LICENSE)
