---
topic: Government of Alberta AI modernization program — timeline, structures, reusable artifacts
volatility: check-quarterly
related: enterprise-ai-lanes.md, canadian-data-requirements.md, canadian-ai-adopters.md
---

# Alberta Velocity program (the template to lift)

Deep extraction from thevelocitywhitepapers.com + github.com/GovAlta + albertaaiacademy.com. (as of 2026-07-11; papers published 2026-07-07). All numbers are Alberta's first-party figures — cite as "Alberta's reported results, to validate in our own pilots."

## The program timeline (the transferable spine)

| When | What | Depends on |
|---|---|---|
| Fall 2024 | Business case: $2B / 130-year modernization estimate framed as burning platform | — |
| **Mar 2025** | **"AI Maximalist" volunteer cohort** (~65 people, ~5% of workforce) starts vibe-coding; produces 1,000+ prototypes and exposes the need for guardrails | culture before platform |
| **Sept 9, 2025** | **AI Academy launches** (3 levels), formalizing what volunteers learned | Maximalist lessons |
| Late 2025 | **Estate scan**: Git Insights reads 466M LOC / 3,400 repos with ~50 agents in ~20 hrs; G.I.M. synthesizes portfolio views (185 apps → 16 modules in one ministry) | frontier-model capability jump |
| Nov 2025 → Feb 2026 | **AI Factory built**: Pronghorn (design) → Nexus (runtime; 600 apps since Feb) → Velocity Game Engine (measurement) | scan + harness + trained people |
| Jan–Jun 2026 | **Proof deliveries**: 5-month app rebuilt in 4 days; ACIP shipped in 11 wks / $108K vs $1.3–1.9M estimate | factory |
| Jul 2026 | **Open-source everything** (papers + tools, MIT); fall 2026: decentralize to "builder culture" | proof |

The two non-obvious ordering choices, both cheap to copy: **(1) volunteer cohort before any platform, (2) automated estate reconnaissance before consolidation planning.**

## AI Academy (training template)

- Entry bar: self-selection + open-mindedness only; no technical background. Mastery ≈ 2–3 months of consistent practice. 2,000+ public servants trained; cohorts of 60–several hundred, all seniority levels mixed.
- **Level 1 (1 week):** prompting — RICECO (Role, Instruction, Context, Examples, Constraints, Output) + "verify, then trust." ~9/10 satisfaction.
- **Level 2:** reusable agents — the **rule of three** (automate anything repeated 3+×/week, share it). Expected fall-off is fine.
- **Level 3:** enterprise builds with the harness. Personas (lateral, not ranked): **warrior** (ship fast), **wizard** (modernize legacy), **craftsperson** (build enablement tooling). Proof: 100 students → **560 working compliant apps in 6 days**.
- Free exec on-ramp to reuse: 2-hr masterclass at masterclass.albertaaiacademy.com (+ 28-min summary, study guides, live workflow demos).

## The harness (what's actually in it)

- Structure: `CLAUDE.md` entry point + `.claude/skills/` instruction files + **hooks** (auto-triggers) + **evals** (checks), on the Claude Agent SDK (Opus for long-horizon, Sonnet for parallel). Hand-curated — never AI-generated (the "photocopier problem").
- **Masterpiece template** ("the best specification is code"): initializes with **773 passing tests**, ~95 security controls, OWASP ASVS L2 + WCAG 2.1 AA pre-resolved, 6 security scanners. Stack: Vue 3/Vite, Node/Express, PostgreSQL, OAuth via Entra/Google, CSRF/refresh-token rotation/centralized error handling baked in.
- **Four review agents**, running continuously (400+ checks/pass): **Green** (deterministic code hygiene), **Yellow** (writing quality / 12 "AI-tell" rules), **Red** (adversarial recon/kill-chains), **Blue** (standards compliance → severity-ranked exec summary page).
- Anti-drift: 7 watcher helpers (Chain Walker, Fingerprint Heatmap, Commit Spine, Drift Sweep…) — separate cheap observer agents leave notes for the builder agent. Harness updates ship as git patches; agents pull current rules via OpenAPI.

## Operating model

- Central ministry (est. 2022) serves 27 ministries: **"fortress stays central"** (cyber, identity, network, data) while domain experts build on governed platforms — shift from centralizing *delivery* to centralizing *governance*.
- Delivery units: 8–12 people traditionally → **1–2 people** with the factory; onboarding 1–2 days (standards carry knowledge, not individuals).
- Intake: every system triaged **tolerate / invest / migrate / eliminate** → routed to AI Garage (fix in place), AI Factory (rebuild), AI Rationalization (consolidate), or Government 3.0 (agent-native).
- Access: agents reach ERP/ServiceNow/SharePoint via gateways with PIM-style access re-delegated every few hours.

## Measurement

- **Three-pillar scorecard:** Readiness (education, tool control, self-sufficiency, governance) · System health (vulnerability reduction, backlog shrinkage — ~600-app backlog falling YoY is the clearest signal) · Cost (direct IT, ministry program impact, workforce-flat-while-output-grows over 3–5 yrs). Principle: "do more for less."
- **Velocity Game Engine:** 8-stage board (requirements → deployment), one skill per stage, mechanical gates, points lost permanently on rework, chess-clock exposing human-vs-AI turn time (handoff stalls usually human), leaderboard, and "the Reaper" anti-gaming agent.

## What you can clone today (github.com/GovAlta, MIT)

- **pronghorn-blue** — design/ideation platform (Azure/OpenAI flavor, built with Microsoft Canada); **pronghorn-red** — original (React/Supabase, unmaintained; live at pronghorn.red).
- **velocity-game-engine** — the gamified PM tool.
- **the-velocity-white-papers** — site source; **ui-components**, **nx-tools** (Apache-2.0).
- **Not published:** Git Insights, G.I.M., Nexus (internal; rebuild from paper descriptions if needed). Watch the org for more releases.

## Numbers for business cases (first-party, sourced in papers)

- Estate: 466M LOC, 3,400 repos, 1,400 systems; scan in ~20 hrs vs ~6.5 yrs manual; 40% first-release standards compliance; 1,280 undocumented repos (agents wrote the docs).
- Delivery: 25× (4 days vs 5 months); ACIP $108K vs $1.3–1.9M; 1,000+ apps program-wide; prototyping ~6 wks → ~30 min.
- Security: remediation ~$1–2/vulnerability; 10,000+ vulns surfaced; 271 Firefox bugs found in one month (10× prior methods).
- Economics: compute "hundreds of thousands/month" but single-app cost avoidance frequently exceeds a year of AI compute; RAG benchmarking project ~$200; 42,000-page curriculum pipeline ~$2,500.

Key sources: paper index at thevelocitywhitepapers.com (individual /paper/ pages render text; index pages are JS-only) · https://github.com/GovAlta · https://www.anthropic.com/news/alberta-government-claude-cybersecurity · masterclass.albertaaiacademy.com
