---
topic: OpenAI Codex — coding-agent tool (surfaces, licensing, models, deployment, admin)
volatility: check-monthly
related: ../platforms/chatgpt-enterprise.md, ../canada-residency-paths.md, ../model-pricing.md, ../enterprise-ai-lanes.md
---

# OpenAI Codex

> Per-tool card. Platform/billing context: [../platforms/chatgpt-enterprise.md](../platforms/chatgpt-enterprise.md). Residency/deployment detail: [../canada-residency-paths.md §3.3](../canada-residency-paths.md). Token rates: [../model-pricing.md](../model-pricing.md).

## Surfaces

- One product across: **Codex cloud** (hosted agent in ChatGPT web/iOS, Code Review, Slack/Linear), **Codex CLI** (with GitHub integration), **IDE/VS Code extension**, and the **Codex app**. (as of 2026-07-14, source: https://developers.openai.com/codex/pricing)
- **ChatGPT integration:** Codex is bundled in every paid ChatGPT tier — "ChatGPT Work and Codex are included in your Free, Go, Plus, Pro, Business, Edu, or Enterprise plan." (as of 2026-07-14, source: https://learn.chatgpt.com/docs/pricing)

## Licensing & effective cost

- **Bundled in ChatGPT seats**; no separate Codex license. Since 2026-04-02, consumption is **token-based workspace credits** (1 credit ≈ $0.04) mapping 1:1 to API prices, applied to Plus/Pro/Business/new Enterprise. A **Codex-only seat type** exists on Business (cheaper than full seat). (as of 2026-07-14, sources: https://help.openai.com/en/articles/20001106-codex-rate-card, https://www.eesel.ai/blog/codex-pricing)
- Individual entry: **Plus $20/mo** (10–60 cloud tasks + 20–50 reviews per 5-hr window); **Pro $100/mo** (5×) / **20× tier** for heavy users. (as of 2026-07-14, source: https://www.eesel.ai/blog/codex-pricing)
- **Effective heavy use ≈ $100–200/dev/mo** (OpenAI's own rate-card guidance for Codex-as-primary-teammate; repo deck budgets $150 midpoint → ~$27k/yr for 15 devs). Credits draw from the **shared org pool** — set spend controls before it drains. (as of 2026-07-14, source: https://help.openai.com/en/articles/20001106-codex-rate-card)

## Models it runs

- Primary: **GPT-5.6 family — Sol / Terra / Luna** (credits per MTok in/cached/out: Sol 125/12.5/750, Terra 62.5/6.25/375, Luna 25/2.5/150), plus **GPT-5.5, GPT-5.4, GPT-5.4-mini**, and **GPT-5.3-Codex-Spark** (research preview, Pro only, non-final rates). Avg 5–40 credits/message on 5.6. (as of 2026-07-14, source: https://learn.chatgpt.com/docs/pricing)
- **Correction vs corpus:** the ChatGPT Codex product's default coding models are now the **GPT-5.6 family**; standalone **`gpt-5.3-codex` is no longer a listed Codex product model** (superseded by 5.6 + Codex-Spark). `gpt-5.3-codex` remains **deployable on Azure Foundry canadaeast** ([../canada-residency-paths.md §3.5](../canada-residency-paths.md)), so it persists as a *deployment* target, not the ChatGPT-Codex default. (as of 2026-07-14, source: https://learn.chatgpt.com/docs/pricing)

## Config / deployment options

- **Azure/Foundry:** Codex CLI runs against Azure OpenAI/Foundry deployments via documented `config.toml` provider setup — point at a **canadaeast Global Standard** deployment for CA-at-rest / US-inference under an existing Microsoft agreement. (as of 2026-07-14, source: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/codex)
- **Amazon Bedrock:** supported as a Codex model provider since Jun 2026. (as of 2026-07-14, source: https://techpp.app/update-tracker/openai-codex-changelog)
- **OpenAI API path:** `ca.api.openai.com` = Canada storage-only region (data at rest CA, inference US), per-Project config, ZDR amendment available. (as of 2026-07-14, source: https://developers.openai.com/api/docs/guides/your-data)
- **Residency:** Codex **honors ChatGPT workspace data residency** (backend enforces region auth for non-US workspaces). What Codex-cloud artifacts (repos, containers, task logs) store in-region for a Canada workspace is **not publicly documented — get it in writing** (deck Pre-signature item #1). (as of 2026-07-14, source: https://developers.openai.com/codex/enterprise/admin-setup)

## Admin controls

- Governed under ChatGPT Enterprise/Business admin: Global Admin Console, role-based enablement, usage/credit visibility, Compliance API audit trails, spend/credit caps per team. (as of 2026-07-14, source: https://intuitionlabs.ai/articles/chatgpt-enterprise-admin-controls-security)

## Comparison hooks (facts only)

- **vs Claude Code:** Claude Code meets CA-at-rest/US-inference today via Bedrock ca-central-1 (Alberta pattern); Codex meets it via Enterprise Canada workspace or CLI→Azure canadaeast. Claude Code leads recent SWE-bench Pro / dev "most loved" surveys (~46% vs ~9% Copilot). Both bill by token consumption. (as of 2026-07-14, source: ../enterprise-ai-lanes.md, ../canada-residency-paths.md)
- **vs GitHub Copilot:** Copilot Business $19 / Enterprise $39 + usage credits; went usage-based Jun 2026. Codex has no flat coding seat — it rides the ChatGPT seat + shared credit pool. (as of 2026-07-14, source: ../enterprise-ai-lanes.md)

## What's still unverified

- In-region storage scope for Codex-cloud artifacts in a Canada workspace (sales letter).
- Whether `gpt-5.3-codex` returns as a named Codex product model or stays Azure-only.
- GPT-5.3-Codex-Spark final credit rates (currently non-final, Pro preview).
- CAD pricing; whether Sonnet-style intro discounts have any Codex analog.
- Real Enterprise credit-pool baseline included per seat before overage billing.
