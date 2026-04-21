---
name: pdw-talk
description: Product Dev Workflow — roundtable discussion. Use when the user types /pdw-talk [topic] for market analysis, ideation, architecture discussion, or any topic that does not require writing code.
---

# /pdw-talk — Roundtable Discussion

**Language rule:** Always reply in the same language the user writes in. Translate all output, role labels, section headers, and prompts to match.

**Model: 🟡 Sonnet**
**Purpose:** Market analysis, ideation, architecture discussion — no development required.

## ⚠️ Switch Model First

Prompt the user to switch to Sonnet before starting. Translate the prompt to the user's language.

- Claude Code → `/model sonnet`
- Claude.ai → Click model name → Select Sonnet

## Fixed Roles (always present)

- 📋 Product Manager — requirements, priorities, alignment
- 🏗 Technical Architect — feasibility, architecture
- 👤 End User — real pain points, usability

## Dynamic Roles (31 total — auto-select 2–4 by topic)

**C-Suite:** 👔 CEO · 🛠 CTO · 💼 COO · 💰 CFO · 🎨 CPO · 📣 CMO
**Management:** 🏗 Engineering Manager · 📊 Data Lead · 🔐 CISO · 🤝 Partnership Manager
**Specialists:** 🎨 UX Designer · ⚖️ Legal · 🧪 QA · 📊 Business Analyst · 📊 Data Engineer · 🔬 Data Scientist · 🔐 Security Engineer · 🏛 DevOps/SRE · 💰 Finance · 👤 Customer Success · 🎯 Growth · 🤝 Sales · 🌍 Localization · ♿ Accessibility · 📱 Mobile Engineer · 🤖 AI Engineer · 📣 Marketing
**External:** 🏢 Enterprise Client · 🔍 Competitor Analyst · 💡 Industry Advisor

## Auto-Selection by Topic

| Topic | Auto-invite |
| --- | --- |
| New product | CEO, CPO, CMO, Marketing, Finance |
| Major version | CEO, CTO, CPO, Engineering Manager |
| Architecture | CTO, Engineering Manager, DevOps |
| Business model | CEO, CFO, CMO, Business Analyst |
| AI features | AI Engineer, CTO, Legal, Security |
| User data | CISO, Security, Legal, Data Engineer |
| Go-to-market | CEO, CMO, CPO, Sales, Marketing |
| Market research | Competitor Analyst, Industry Advisor, CMO |
| Mobile / App | Mobile Engineer, UX Designer, QA |
| B2B | CEO, Sales, Partnership, Enterprise Client |
| Internationalization | Localization, CMO, Legal |
| Security / compliance | CISO, Security Engineer, Legal |

## Confirmation Before Starting

Show proposed attendees and ask for confirmation. Translate to user's language.

## Output Format

All section labels and content in user's language:

```
📄 [ROUNDTABLE SUMMARY / 圓桌會議摘要]
────────────────────────────────
Topic / 主題     :
Attendees / 與會者:

[Role] Perspective:
  ...

Consensus / 共識      :
Action Items / 行動項目:
Open Questions / 待議  :
────────────────────────────────
```

After output, ask (in user's language):
- Save summary to `.claude/records/`?
- Start a development branch for any action item?
