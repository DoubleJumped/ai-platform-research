---
topic: GPT-5.6 Sol/Terra/Luna — Artificial Analysis intelligence & cost-per-task data
volatility: check-monthly
related: 00-guide.md, ../model-pricing.md, ../model-families.md
---

# Cost per intelligence — Artificial Analysis data (GPT-5.6 family)

Primary decision metric: **AA "Cost per Task"** — input+cache+reasoning+answer token cost divided by task count across the Intelligence Index suite (as of 2026-07-14, source: https://artificialanalysis.ai/evaluations/artificial-analysis-intelligence-index). Don't conflate with "cost to run full eval" (whole-suite total).

## Headline numbers (each tier at max reasoning)

| Model (max) | Intelligence Index | Coding Agent Index | Cost/Task | Output tokens (full eval) | Speed tok/s | II points per $ |
|---|---|---|---|---|---|---|
| **Sol** | 59 | **80 (SOTA #1)** | $1.04 | 70M (least verbose) | 66 | ≈57 |
| **Terra** | 55 | 77.4 | $0.55 | 96M | 137 | ≈100 |
| **Luna** | 51 | 74.6 | $0.21 | 130M (most verbose) | 216 | **≈243** |
| Claude Fable 5 | 60 (#1) | 77.2 | ~$3.12 *(derived)* | 87M | 59 | ≈19 |
| Claude Opus 4.8 | 56 | 72.5 | not published | 120M | 55 | — |

(as of 2026-07-09, source: https://artificialanalysis.ai/articles/gpt-5-6-has-landed and per-model pages https://artificialanalysis.ai/models/gpt-5-6-sol / -terra / -luna; Fable cost/task derived from AA's "Sol ≈ one-third of Fable 5 per task" — estimate, not AA-published)

Sol effort-level Intelligence Index points (only sub-max points extractable): **max 59 / xhigh 58 / high 56**. Per-effort cost/task curves live only in AA's interactive chart — not extractable as text. (as of 2026-07-14, source: https://artificialanalysis.ai/models)

## The three facts that decide everything

1. **Terra is off the Pareto frontier.** AA verbatim: "Luna and Sol are always on the Pareto frontier ahead of Terra. For any Terra effort level, there is a Luna or Sol effort level that is more intelligent at no extra cost, or as intelligent at lower cost." (as of 2026-07, source: https://artificialanalysis.ai/articles/gpt-5-6-intelligence-vs-cost-across-sol-terra-luna)
2. **Verbosity inversion.** Cheaper tiers burn MORE tokens for the same work: Sol 70M → Terra 96M → Luna 130M. "Minimal tokens" and "minimal cost" point in opposite directions — Sol wins on tokens/latency/context budget, Luna wins on dollars.
3. **Sol is the value flagship, not the luxury option.** 59 II is within 1 point of Fable 5 (60, #1) at ~⅓ the cost per task, ~2× the speed, ~45% fewer output tokens.

## Value ratios

- Luna is ~4.3× more cost-efficient per II point than Sol, ~12× more than Fable 5, while sitting 8–9 II points lower.
- Trilogy's independent estimate corroborates: "Luna ~24 benchmark points per API dollar vs Fable 3.2." (as of 2026-07, source: https://trilogyai.substack.com/p/gpt56-terra-luna-and-sol-gain-a-powerful)
- Ultra mode: ~3× cost for ~3 points (Terminal-Bench 88.8→91.9) — correctness-critical work only. (source: https://www.vellum.ai/blog/gpt-5-6-benchmarks-explained)

## Known gaps (flagged, not guessed)

- Per-effort cost/task for each tier: not published in fetchable text (JS chart only).
- Luna full-eval total: not published; rough bound ~$0.9–1.0k from token counts (estimate).
- Opus 4.8 / Gemini 3.5 Flash cost/task: not published by AA.

Implication: the guide's effort-level advice below the max tier rests on OpenAI docs + practitioner reports ([04-community-signal.md](04-community-signal.md)), not AA numbers. Revisit when AA exposes per-effort data.
