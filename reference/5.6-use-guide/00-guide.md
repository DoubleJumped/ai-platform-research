---
topic: GPT-5.6 Sol/Terra/Luna — which model at which effort for which dev task (the decision guide)
volatility: check-monthly
related: 01-cost-per-intelligence.md, 02-copilot-economics.md, 03-official-positioning.md, 04-community-signal.md, ../model-pricing.md
---

# GPT-5.6 use guide — Sol / Terra / Luna for dev work

The answer first. Synthesized 2026-07-14 from Artificial Analysis data, OpenAI docs, GitHub Copilot docs, and practitioner signal (Theo/t3.gg, Simon Willison, Every.to, Raschka, Vellum, dev forums). Evidence in files [01](01-cost-per-intelligence.md)–[04](04-community-signal.md). Deck: `decks/gpt-5.6-model-use-guide.html`.

## The one-paragraph version

Run a **barbell, not a ladder**: **Luna for the small stuff, Sol for the hard stuff, and skip Terra by default** — Artificial Analysis shows every Terra config is beaten by some Sol or Luna config on the intelligence-vs-cost frontier. The effort dial is available everywhere we work: **VS Code Copilot** (model picker → arrow → Thinking Effort), Codex CLI (`--effort`), and the API. The working default is **Sol Medium, escalating to Sol High for anything hard or longer than ~10 minutes**; below that, prefer **Luna at high effort over Terra** for cheap work (Raschka: "Luna xhigh may be better AND cheaper than Sol Medium"). Keep **Max/Ultra as break-glass** (~2× cost for ~4 pts, per Theo's curve; Ultra isn't in Copilot at all). Plan/architect with your highest-judgment model, implement with Sol — and always give 5.6 explicit stop conditions, because its defining failure mode is not stopping.

## Decision matrix — model + effort (VS Code Copilot / Codex CLI / API)

| Dev task | Use | Why (evidence) |
|---|---|---|
| Spot edits, renames, known-error fixes, quick unit tests | **Luna high** (or Sol low if latency matters) | Luna ≈243 II-pts/$ vs Sol 57 [01]; effort guides map low-effort Sol here [04] |
| Everyday features, API integration, routine multi-file work | **Sol medium** | OpenAI's default + "test one level lower" rule [03]; aiidelist/agiflow [04] |
| Complex debugging, race conditions, big PR review | **Sol high** | Theo's default for >10-min tasks; "only good quadrant of cost-to-intelligence" [04] |
| Long agentic runs, terminal-heavy, computer-use | **Sol high** | Terminal-Bench 88.8 SOTA; week-long 150k-LOC report, no drift [03][04] |
| Planning / architecture / scoping | **Sol xhigh** — or better, a high-judgment model (Fable 5), then hand Sol the plan | Sol over-builds: 56/100 vs Fable 90/100 on Every.to's architecture rubric; "Porsche vs warp drive" [04] |
| Repo-wide SWE-bench-style changes | Consider **Fable 5/Opus** if available | SWE-Bench Pro: Fable 80% vs Sol 64.6% — Sol's one clear gap [03] |
| One brutally hard interconnected problem | **Sol max** (sparingly) | +4 pts over high for >2× cost — pay it only when correctness ≫ budget [04] |
| Large parallelizable audits/migrations | **Sol xhigh** first; **Ultra** only if truly parallel | Practitioners prefer xhigh — fewer subagents, easier steering; Ultra drains weekly quotas in hours [04] |
| Bulk classification / summarization / batch pipelines | **Luna** (orchestrated, batched) | $0.21/task; but see red line below [01] |

## Copilot mechanics (VS Code)

The matrix above applies directly in VS Code — the Thinking Effort submenu lives behind the arrow next to the model name in the Copilot picker (default medium, sticks per session). Copilot-specific caveats:

- **No Ultra** — base Sol only; the parallel multi-agent mode is Codex/ChatGPT/API.
- **Custom agents pin models, not efforts** — `.github/agents/*.agent.md` frontmatter takes `model:` but no effort key yet, so "orchestrator plans on Sol high, subagents run Luna xhigh" needs a manual effort flip per session, not a declarative config.
- GPT-5.6 policy is **off by default** for Business/Enterprise (admin must enable); Sol needs Pro+/Business/Enterprise; credits pool org-wide so budget at pool level; inline completions are free; effort bills as more reasoning tokens, no multiplier. Details: [02].

## Red lines (things that will burn you)

1. **Never give Luna long-context recall** — retrieval collapses to 41.3% past 256K (Sol 91.5%) [03].
2. **Watch the 272K-token 2× surcharge** (Copilot long-context and Codex both) — restart sessions instead of growing threads [02][04].
3. **Stop conditions in every agentic prompt** — "Build the plan, then stop for feedback." Theo cut token burn 4–5× this way; 5.6 "is so unlikely to stop that I have to put up the stop signs myself" [04].
4. **Restrain subagents** — one agent.md line: "Only use sub-agents if the user explicitly requests them" [04].
5. **Review every diff** — METR measured Sol's reward-hacking rate as the highest of any public model they've assessed; it exceeds user intent above average [04].
6. **Don't default to Max/Ultra** — OpenAI itself says don't recommend max globally; Ultra ≈ 3× cost for ~3 pts [03][04].
7. **Effort down, not up, when migrating from 5.5** — test one level lower first; 5.6 holds quality with fewer tokens [03].

## Why not Terra? (the contrarian call, stated honestly)

GitHub and Vellum bless Terra as "the balanced default," and it's fine. But: (a) AA's Pareto analysis — "for any Terra effort level, there is a Luna or Sol effort level more intelligent at no extra cost, or as intelligent at lower cost" [01]; (b) verbosity inversion means Terra's list-price discount partly evaporates in extra output tokens (96M vs Sol's 70M on the same eval) [01]; (c) both primary negative field reports were Terra sessions (weekly quota gone in 6 hours; 5-hour window gone in minutes) [04]. Terra's honest niche: one-model simplicity, and plans where Sol isn't available.

## Cost cheat-sheet (per task, AA suite, max effort)

Luna **$0.21** · Terra $0.55 · Sol **$1.04** · Claude Fable 5 ~$3.12. Sol = 59/60ths of Fable's intelligence at ~⅓ the per-task cost — the value flagship. Luna = ~4.3× Sol's intelligence-per-dollar. [01]
