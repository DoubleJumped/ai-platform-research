---
topic: GPT-5.6 Sol/Terra/Luna — official OpenAI positioning, effort levels, benchmarks
volatility: check-quarterly
related: 00-guide.md, 01-cost-per-intelligence.md, ../model-families.md
---

# GPT-5.6 official positioning & effort system

Released 2026-07-09 GA. All three tiers: **1.05M context, 128K max output, cutoff 2026-02-16**. (as of 2026-07-14, source: https://developers.openai.com/api/docs/models)

## The three tiers (OpenAI's words)

- **Sol** — "our new flagship." SOTA coding/agentic/computer-use; Copilot framing: "highest reasoning ceiling… complex reasoning over large codebases and demanding, long-running agentic work."
- **Terra** — "a balanced model for everyday work"; ~GPT-5.5 performance at ~half price.
- **Luna** — "our fastest and most affordable model."
(source: https://openai.com/index/gpt-5-6/; https://github.blog/changelog/2026-07-09-openais-gpt-5-6-sol-terra-and-luna-are-now-available-in-github-copilot/)

Naming: number = generation, Sol/Terra/Luna = durable tiers going forward. (source: https://openai.com/index/previewing-gpt-5-6-sol/)

## Reasoning effort levels (API/Codex/ChatGPT — not Copilot)

All three tiers support **`none / low / medium / high / xhigh / max`**; default `medium`. **`ultra` is a mode, not an effort level** — Responses API multi-agent beta, 4 parallel agents by default. `max` = more reasoning time than `xhigh`. (source: https://developers.openai.com/api/docs/models; https://openai.com/index/gpt-5-6/)

OpenAI's own effort guidance (source: https://developers.openai.com/api/docs/guides/prompt-guidance-gpt-5p6):
- `low` — latency-sensitive, when quality holds
- `medium` — the starting default
- `high`/`xhigh` — only when evals show a real gain
- `max` — hardest quality-first work; "do not recommend it globally"
- **Migration rule: start from your 5.5-era effort, then test one level LOWER** — 5.6 often holds quality with fewer tokens.
- Advisory phase mapping (latest-model guide): planning → Terra/Sol at low-medium; implementation → Sol (esp. frontend) or Terra routine; review/validation → Luna cheap, raise effort only on quality gaps.

## Benchmarks worth citing (headline = max effort)

| Eval | Sol | Terra | Luna | Fable 5 | Opus 4.8 |
|---|---|---|---|---|---|
| AA Coding Agent Index v1.1 | **80 (SOTA)** | 77.4 | 74.6 | 77.2 | 72.5 |
| DeepSWE v1.1 | 72.7% | 69.6% | 67.2% | 69.7% | 59.0% |
| Terminal-Bench 2.1 | 88.8% (Ultra 91.9%) | 87.4% | 84.7% | 83.1% | 78.9% |
| SWE-Bench Pro | 64.6% | 63.4% | 62.7% | **80%** | 69.2% |
| Agents' Last Exam | 52.7% | 50.4% | 50.3% | 40.5% | 45.2% |

(source: https://openai.com/index/gpt-5-6/, fetched 2026-07-14)

**Weaknesses to teach, not hide:**
- **SWE-Bench Pro is Sol's clear gap** — trails Fable 5 by ~15 pts (OpenAI counters ~30% of tasks are broken; contested).
- **Luna's long-context retrieval collapses**: MRCR v2 8-needle 256K+: Luna **41.3%** vs Terra 89.6% / Sol 91.5%. Never give Luna big-context recall work.
- Page prose vs table inconsistency on Agents' Last Exam (53.6 vs 52.7) — likely effort variants.

## Official dev-workflow guidance (prompt-guidance doc)

- **Autonomy boundaries**: let it act on safe local changes; require confirmation for external/destructive/scope-expanding actions.
- **Stop conditions**: define explicit stopping conditions; sparse phase-change updates on long runs.
- **Simplify prompts**: removing redundant instructions/tools reported +10–15% quality with 41–66% fewer tokens.
- **Programmatic Tool Calling** (Responses API): model runs JS in isolated V8, no network; 38–63.5% token cuts reported for batch/filter workflows.
- Caching: explicit breakpoints, 30-min minimum life; **5.6+ cache writes bill 1.25× input**; reads keep the 90% discount.

## Product access

Free/Go → Terra. Plus/Pro/Business/Enterprise → all three with per-model effort. `max` toggle for all with 5.6 access in ChatGPT Work/Codex; `ultra` = Pro/Enterprise (Work), Plus+ (Codex). (source: https://openai.com/index/gpt-5-6/)
