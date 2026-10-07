# Axia Claude Skills

Practical [Claude Code](https://claude.com/claude-code) skills for African businesses, from [Axia Analytics](https://axiaanalytics.com).

This repo has two parts:
1. **Axia skills.** Three original skills built for East African SMEs, SACCOs and finance teams.
2. **Recommended skills.** Five third-party skills we've checked and recommend, with install commands that are current as of **6 October 2026**.

> **What is a skill?** A skill is an instruction file (`SKILL.md`) that teaches Claude how to do a specific job well and consistently. Install it once and Claude uses it whenever the task comes up.

---

## Part 1: Axia skills

| Skill | What it does |
|---|---|
| [`mpesa-statement-analysis`](skills/mpesa-statement-analysis/SKILL.md) | Turns an M-Pesa, Airtel Money or MoMo statement into a clean transaction table, a cash-flow summary, a fee breakdown and plain-English insights. |
| [`monthly-kpi-report`](skills/monthly-kpi-report/SKILL.md) | Builds a one-page monthly management report from your sales, finance or operations data, with KPIs suggested by sector (retail, SACCO, services). |
| [`ai-opportunity-audit`](skills/ai-opportunity-audit/SKILL.md) | Interviews you about your processes, scores them for automation potential and produces a prioritised AI roadmap. |

### Install

In Claude Code, run:

```
/plugin marketplace add Sharonmaganga/axia-claude-skills
/plugin install axia-skills@axia-analytics
```

To install by hand instead, copy any folder from `skills/` into `~/.claude/skills/`.

---

## Part 2: Recommended skills (checked 6 Oct 2026)

All five are actively maintained by their authors. Credit and licences belong to them; we only link to their work.

### 1. Graphify: turn documents and code into a searchable knowledge graph
- **Repo:** [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) (moved from `safishamsi/graphify`)
- **Good for:** large collections of policies, SOPs, PDFs and code that you want to query without re-reading everything.
- **Needs:** Python 3.10+
```bash
pip install graphifyy && graphify install
```
On Windows or macOS, use `pipx install graphifyy` if `pip` gives you trouble.

### 2. UI/UX Pro Max: professional design for websites and apps
- **Repo:** [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
- **Good for:** making client portals, dashboards and landing pages look professional.
```
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
```

### 3. Claude SEO: website audits and fixes
- **Repo:** [AgriciDaniel/claude-seo](https://github.com/AgriciDaniel/claude-seo)
- **Good for:** technical SEO, schema markup, local SEO and optimising for AI search (GEO/AEO). It now has 26 sub-skills and 19 subagents.
```
/plugin marketplace add AgriciDaniel/claude-seo
/plugin install claude-seo@agricidaniel-claude-seo
```

### 4. Remotion: create videos with code
- **Repo:** [remotion-dev/skills](https://github.com/remotion-dev/skills) · [Docs](https://www.remotion.dev/docs/ai/skills)
- **Good for:** product demos, explainer videos and animated data stories.
- **Needs:** Node.js
```bash
npx skills add remotion-dev/skills
```

### 5. Google Workspace CLI: let Claude work in Gmail, Drive, Sheets, Docs and Calendar
- **Repo:** [googleworkspace/cli](https://github.com/googleworkspace/cli)
- **Good for:** automating reports into Sheets, filing Drive documents and drafting emails.
- **Needs:** Node.js 18+ and a Google account. Note that the latest release is v0.22.5 (March 2026) and the tool is still pre-1.0.
```bash
npm install -g @googleworkspace/cli
gws auth setup
npx skills add https://github.com/googleworkspace/cli
```

---

## Need help putting AI to work?

Axia Analytics helps organisations across Africa turn fragmented data into decisions, through AI automation, data engineering and analytics.
**[Start a conversation →](https://axiaanalytics.com/#contact)**

## Licence

The Axia skills in `skills/` are MIT licensed (see [LICENSE](LICENSE)). The third-party skills are covered by their own licences.
