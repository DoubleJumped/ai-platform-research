---
topic: GPT-5.6 Sol/Terra/Luna inside GitHub Copilot — billing, availability, gotchas
volatility: check-monthly
related: 00-guide.md, 01-cost-per-intelligence.md, ../model-pricing.md, ../enterprise-ai-lanes.md
---

# GPT-5.6 in GitHub Copilot — economics

## The billing model (common misconception first)

- Copilot moved to **usage-based billing with AI Credits (1 credit = $0.01 USD)** on 2026-06-01; models bill **per token**, not per premium-request multiplier. (as of 2026-06-01, source: https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/)
- **Sol/Terra/Luna are not in the legacy multiplier table at all** — they are token/credit-billed only. The "$0.04/premium request" framing does not apply to them. (as of 2026-07-14, source: https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- **Inline code completions / next-edit suggestions are not credit-billed** — the only effectively free surface.

## Token pricing in Copilot (per MTok; credits = USD × 100)

| Model | Input | Cached in | Output | Long-context (>272K; Luna >200K) in/out |
|---|---|---|---|---|
| Sol | $5.00 | $0.50 | $30.00 | $10.00 / $45.00 |
| Terra | $2.50 | $0.25 | $15.00 | $5.00 / $22.50 |
| Luna | $1.00 | $0.10 | $6.00 | $2.00 / $9.00 |

Output-cost ratio **Sol : Terra : Luna = 5 : 2.5 : 1**. Anchors: Claude Opus 4.8 $5/$25, Sonnet 5 $2/$10 promo, Gemini 2.5 Pro $1.25/$10. (as of 2026-07, source: https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

⚠️ The **long-context 2× surcharge** past 272K tokens is the silent budget killer on long agent sessions — Theo flagged the same mechanic in Codex. Start fresh sessions rather than letting threads grow.

## What Copilot exposes (and doesn't)

- **VS Code HAS a reasoning dial**: model picker → `>` arrow next to the model name → **Thinking Effort** submenu. Six levels for GPT-5.6 (none/low/medium/high/xhigh/max) per the launch changelog; default **medium** with adaptive reasoning; the setting sticks per model per session. (as of 2026-07-14, sources: https://github.blog/changelog/2026-07-09-openais-gpt-5-6-sol-terra-and-luna-are-now-available-in-github-copilot/ "Six reasoning effort levels… Adjust context size and reasoning effort from a unified picker"; https://code.visualstudio.com/docs/agent-customization/language-models)
  - Doc-lag flag: the generic VS Code doc still lists only none→high; the changelog's six-level claim for 5.6 is not screenshot-confirmed. Verify in your own picker.
  - Old settings keys (`github.copilot.chat.responsesApiReasoningEffort` etc.) are deprecated; the picker is the supported path.
- **Copilot CLI**: `--effort low|medium|high|xhigh` (or `effortLevel` in ~/.copilot/config.json); no none/max. (source: https://github.com/github/copilot-cli/issues/2904)
- The dial is documented for **VS Code + CLI only**; other surfaces (Visual Studio, JetBrains, web, cloud coding agent) get the models without a documented effort control.
- **No Ultra mode in Copilot** — base Sol only; Ultra multi-agent lives in Codex/ChatGPT Work/Responses API.
- **Custom agents (`.github/agents/*.agent.md`) can pin a model, NOT an effort** — effort is session-global. Feature requests open (microsoft/vscode #313546, copilot-cli #2904), unimplemented as of 2026-07-14. So "this subagent = Luna xhigh" is not declaratively configurable in Copilot yet.
- Effort billing: higher effort = more reasoning tokens billed as output at list price. **No effort multiplier.**
- **Sol requires Pro+/Max/Business/Enterprise** — not base Pro. Terra and Luna on all paid plans.
- **Business/Enterprise: GPT-5.6 policy is OFF by default** — an admin must enable it or nobody sees the models. (as of 2026-07-09, source: https://github.blog/changelog/2026-07-09-openais-gpt-5-6-sol-terra-and-luna-are-now-available-in-github-copilot/)
- Auto model selection earns a ~10% discount.

## Plan economics

| Plan | $/user/mo | Included credits/mo |
|---|---|---|
| Business | $19 | 1,900 (promo 3,000 to 2026-09-01) |
| Enterprise | $39 | 3,900 (promo 7,000 to 2026-09-01) |

- **Credits pool at the billing entity** — 100 Business seats = one 190,000-credit pool. A few Sol-heavy devs are absorbed by lighter users; budget at pool level, not per seat.
- Overage: effectively $0.01/credit pay-as-you-go (inferred — docs don't print the line item; one aggregator claimed $0.04/credit — unverified, flag). (as of 2026-07, source: https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- Agent-mode sessions bill **all session tokens**, so long multi-file runs cost regardless of prompt count.

## Worked example — 300 requests/mo/dev (assumed ~15K in + 3K out tokens/request; illustrative only)

| Mix | Cost/request | Month | vs 1,900-credit pool share |
|---|---|---|---|
| Sol-heavy | $0.165 (16.5 cr) | $49.50 | ~2.6× one seat's share |
| Terra-heavy | $0.083 (8.3 cr) | $24.75 | ~1.3× |
| Luna-heavy | $0.033 (3.3 cr) | $9.90 | comfortably inside |

Implication: in Copilot you have **two levers — model (a 5× price swing) and Thinking Effort (a token-volume swing)**. The cost-optimal play per [01-cost-per-intelligence.md](01-cost-per-intelligence.md) is the Luna↔Sol barbell with effort at medium, raised to high only for hard work; Terra is the "don't want to think about it" middle.
