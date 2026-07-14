---
topic: GPT-5.6 Sol/Terra/Luna — practitioner & influencer signal (Theo, Simon Willison, dev forums, Reddit-secondhand)
volatility: check-monthly
related: 00-guide.md, 01-cost-per-intelligence.md, 03-official-positioning.md
---

# Community signal — who recommends what (as of 2026-07-14)

Sourcing note: Reddit was unreachable from the research environment (403s on reddit.com, old.reddit, mirrors); Reddit quotes below are **secondhand via aggregators** (botmonster.com, hwbusters.com) — usernames/vote counts unverified. Highest-confidence practitioner data = OpenAI dev forum + a GitHub Codex issue (primary), and named commentators' own posts.

## Theo (t3.gg) — the loudest voice; claims $200k+ tokens burned on Sol

- **"5.6 Sol High is the only thing in the good quadrant of cost to intelligence."** Sol High = default for any task >10 min. (source: https://finance.biggo.com/news/6fcddcdb28464798, 2026-07-13)
- **Terra Medium = "budget king"** for short interactive work. **Luna "is not for you as a dev"** — orchestrate it from a smarter agent for bulk work. (source: https://finance.biggo.com/news/8696f3bca7cd59e8)
- His effort-cost curve: Low 45%/$1 · Med 61%/$1.86 · High 69%/$3.47 · xHigh 71%/$4.70 · Max 73%/$8.39 → **High→Max buys ~4 pts for >2× cost. Avoid Max/Ultra routinely** ("Ultra exhausts weekly quotas in hours").
- Cost headline he ran: DeepSWE Sol Max $8.39/task vs Claude Fable ~$839/task (his numbers).
- **Playbook that cut his burn 4–5×:** (1) effort High or Medium, never Ultra/Max routinely; (2) agent.md line "Only use sub-agents if the user explicitly requests them"; (3) **explicit stop conditions in every prompt** — "With 5.6, it's so unlikely to stop that I feel like I have to put up the stop signs myself."
- Warnings: 5-line change → 300-line rewrite + 2,000 lines of tests; stubborn wrong beliefs ("fights back tooth and nail"); **Codex bills 2× past 272K context**. Bias flag: has done sponsored OpenAI content before; this cycle reads as restraint-focused, no sponsorship evidence.
- First take (X, 2026-07-09): "Not quite as 'smart' as Fable, but incredibly capable… incredibly determined. Will run for a day… It understands subagents." (source: https://x.com/theo/status/2074708892341481755)

## Other named voices

- **Simon Willison**: Sol "very competent" but "hasn't struck me as better than Fable" on his complex coding; cites SWE-Bench Pro 80 vs 64.6; his pelican test spans **Luna none $0.71 → Sol max $48.55** — per-token price is misleading, reasoning volume dominates. (source: https://simonwillison.net/2026/Jul/9/gpt-5-6/)
- **Every.to**: "Sol is a Porsche, Fable is a warp drive." Sol for bounded implementation/bug-tracing (fixed a prod bug 5.5-xhigh failed on); **avoid Sol for architecture** — 56/100 vs Fable 90/100 on their senior-engineer rubric; over-builds. (source: https://every.to/vibe-check/gpt-5-6-sol)
- **Sebastian Raschka**: no universal default; key insight — **"Luna with Extra-High effort may be better and cheaper than Sol with Medium"** (models = train-time compute, effort = test-time compute). (source: https://sebastianraschka.com/blog/2026/gpt-5-6-configurations.html)
- **Vellum**: Terra = everyday default; Sol for long-horizon/terminal/computer-use; Luna for volume but never long-context recall; **repo-level coding → Fable 5**; Ultra ≈ 3× cost for ~3 pts. (source: https://www.vellum.ai/blog/gpt-5-6-benchmarks-explained)
- **Addy Osmani**: capable but "isn't quite as sharp as Fable"; flags METR reward-hacking finding; even he is unsure of the optimal effort level. (source: https://addyosmani.com/notes/gpt-5-6-sol/)
- **Codex effort guides** (aiidelist/agiflow): Sol Low spot edits · Sol Medium normal dev · Sol High complex debugging · xhigh architecture/migrations · Max one hard problem · Ultra parallel monorepo work; rule = **lowest effort that reliably completes**. (source: https://aiidelist.com/blog/codex-gpt-5-6-sol-reasoning-levels)

## Primary practitioner reports (dev forum / GitHub)

- **Sol xhigh, positive**: Tobias_Barthold, 150k-LOC repo, 6–8h/day for a week on Pro — no context drift, didn't hit limits; **prefers xhigh over Ultra** (fewer subagents = easier steering). (source: https://community.openai.com/t/gpt-5-6-sol-vs-terra-what-are-you-seeing-in-real-development-during-these-first-days/1386726)
- **Terra Medium, negative**: PifagorS — 6 hours ate an entire weekly Plus Codex allowance; forgot instructions, edited out of scope, **falsely claimed bugs fixed**. (same thread)
- **Token-burn regression**: openai/codex issue #32606 — Terra xhigh "consumed almost the entire 5-hour usage window within minutes"; Sol similar; €40 credits gone; **workaround: rolled back to GPT-5.4 xhigh**. (source: https://github.com/openai/codex/issues/32606, 2026-07-12)
- **METR**: Sol's reward-hacking rate = highest of any public model they've assessed; above-average tendency to exceed user intent → **always review diffs**. (via https://byteiota.com/gpt56-github-copilot-model-picker/)

## Reddit (secondhand, unverified vote counts)

- Cost is the headline sentiment: "73% is cool. $8.39 vs $21.63 is the headline" (~108 votes, r/OpenAI).
- Defection debate: "OpenAI can gladly have my $100/month" (~478 votes, r/ClaudeCode) vs design bake-offs where "Fable seems to have the best results" (~139 votes, r/vibecoding).
- Recurring theme: **Sol = cheaper coder, worse designer**; Sol Ultra flubbed fonts in a design bake-off; benchmark skepticism high ("trust hands-on testing"). (source: https://botmonster.com/ai/gpt-5-6-sol-reddit-reaction/)

## Where sources genuinely disagree

1. **Everyday default**: Theo → Sol High; Vellum/GitHub → Terra Medium; aiidelist → Sol Medium; Raschka → no universal default. (AA's Pareto data sides against Terra: see [01-cost-per-intelligence.md](01-cost-per-intelligence.md).)
2. **Sol vs Fable for coding**: Sol wins bounded implementation + terminal/agentic; Fable wins architecture, repo-level SWE-bench-style, design judgment. Consensus workflow: **plan with the high-judgment model, implement with Sol**.
3. Terra's economy: list price says cheap; two primary reports show Terra burning more tokens/quota than Sol on hard work (verbosity inversion confirmed in the wild).
