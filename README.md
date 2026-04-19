# product-dev-workflow

> A flexible Claude Skill for the full product & software development lifecycle.

[繁體中文](#繁體中文) | **English**

---

## What is this?

A Claude Skill (`SKILL.md`) that guides Claude through a structured, flexible product development workflow — from market evaluation to system development.

Designed for: indie developers, product managers, and small teams who use Claude as a development partner.

## Features

- 🌐 Bilingual support — Traditional Chinese & English
- 🔀 Flexible entry points — start at any phase based on task size
- 🤖 Model guidance — tells you when to use Opus vs Sonnet vs Haiku
- 📄 Auto document output — Product Brief, Roundtable Summary, PRD, Technical Spec, and more
- 🐛 Quick Fix Track — separate flow for bug fixes and optimizations

## Workflow Overview

```
Phase 1 │ Product Planning & Market Evaluation   🔴 Opus
Phase 2 │ Roundtable Discussion (multi-role)     🟡 Sonnet
Phase 3 │ Execution Planning (per department)    🟢 Sonnet
Phase 4 │ System Development                     🟢 Sonnet
Phase 5 │ Next Version Planning                  🔴 Opus

Quick Fix Track │ Bug Fix / Optimization         🟢 Sonnet
```

You don't need to run all 5 phases every time. The skill determines the right entry point based on your task.

## When to Start at Each Phase

| Scenario | Entry Phase |
|---|---|
| Brand new product | Phase 1 |
| Major version (v2.0+) | Phase 1 |
| Feature affecting architecture | Phase 1 or 2 |
| Medium feature (cross-team) | Phase 2 |
| Small feature | Phase 4 |
| Bug fix / Optimization | Quick Fix Track |

## Model Guide

| Model | When to Use |
|---|---|
| 🔴 Opus | Phase 1 & 5, hard decisions, architecture, mysterious bugs |
| 🟡 Sonnet | Phase 2, 3, 4 — the main workhorse |
| 🔵 Haiku | Boilerplate, renaming, simple lookups |

## How to Install

1. Copy `SKILL.md` into your Claude skills folder
2. Or paste the contents directly into a Claude Project as a custom instruction
3. Start a new conversation and follow the initialization prompt

## How to Use

At the start of each session, Claude will ask:
1. Your preferred language (Chinese / English)
2. What you want to do (new product / new feature / bug fix / etc.)

Then follow the guided flow. Claude will auto-produce documents at the end of each phase and ask if you want to continue to the next.

## License

[Apache License 2.0](./LICENSE)

---

## 繁體中文

### 這是什麼？

一個 Claude Skill（`SKILL.md`），引導 Claude 執行結構化、彈性的產品開發流程，從市場評估到系統開發全流程覆蓋。

適合：獨立開發者、產品經理、小型團隊。

### 功能特色

- 🌐 雙語支援 — 繁體中文 & 英文
- 🔀 彈性入場點 — 根據任務大小從任意 Phase 開始
- 🤖 Model 建議 — 自動提示何時用 Opus、Sonnet、Haiku
- 📄 自動產出文件 — Product Brief、圓桌會議結論、PRD、技術規格書等
- 🐛 快速修復通道 — Bug 修復與優化的獨立流程

### 流程概覽

```
Phase 1 │ 產品規劃 & 市場評估     🔴 Opus
Phase 2 │ 圓桌會議（多角色）      🟡 Sonnet
Phase 3 │ 各部門建立執行計畫      🟢 Sonnet
Phase 4 │ 系統開發                🟢 Sonnet
Phase 5 │ 下一版規劃              🔴 Opus

快速通道 │ Bug 修復 / 優化        🟢 Sonnet
```

### 入場點選擇

| 情境 | 入場 Phase |
|---|---|
| 全新產品 | Phase 1 |
| 大版本（v2.0+）| Phase 1 |
| 影響架構的大功能 | Phase 1 或 2 |
| 中型功能（跨部門）| Phase 2 |
| 小功能 | Phase 4 |
| Bug 修復 / 優化 | 快速通道 |

### 安裝方式

1. 將 `SKILL.md` 複製到你的 Claude skills 資料夾
2. 或直接貼到 Claude Project 的自訂指令中
3. 開啟新對話，跟著初始化提示操作即可

### 授權

[Apache License 2.0](./LICENSE)
