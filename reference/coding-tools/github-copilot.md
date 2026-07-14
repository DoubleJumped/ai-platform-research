---
topic: GitHub Copilot — enterprise reference (plans, model roster, governance, agents)
volatility: check-monthly
related: ../5.6-use-guide/02-copilot-economics.md, ../enterprise-ai-lanes.md
---

# GitHub Copilot — enterprise operational card

Operational reference for the tool this org currently runs. **Billing/economics live in [02-copilot-economics.md](../5.6-use-guide/02-copilot-economics.md) — this card does not re-derive token pricing, credit math, or the reasoning dial; it covers roster, governance, and surfaces.**

## Plans and what they gate

| Plan | $/user/mo | Included AI credits/mo | Gates |
|---|---|---|---|
| Business | $19 | 1,900 (promo 3,000 to 2026-09-01) | org policies, content exclusion, audit logs, no-training DPA |
| Enterprise | $39 | 3,900 (promo 7,000 to 2026-09-01) | + Copilot Spaces/knowledge, enterprise-wide policy cascade, GHEC integration, 365-day data retention, enforced duplication filter |

- (as of 2026-07-14, source: https://github.com/features/copilot/plans) Usage-based AI Credits (1 credit = $0.01) since 2026-06-01; see economics card for the full model. Both paid plans include the coding agent, agent mode, CLI, and MCP — the plan line mostly gates *governance*, not *features*.

## Model roster (selectable now)

- (as of 2026-07-14, source: https://docs.github.com/en/copilot/reference/ai-models/supported-models)
- **OpenAI:** GPT-5.6 Sol / Terra / Luna, GPT-5.5, GPT-5.4 (+ mini/nano), GPT-5.4 mini, GPT-5.3-Codex, GPT-5 mini.
- **Anthropic (Claude):** Opus 4.8 (+ 4.8 fast mode preview), Opus 4.7 / 4.6 / 4.5, Sonnet 5 / 4.6 / 4.5, Haiku 4.5, Fable 5.
- **Google:** Gemini 3.1 Pro (preview), Gemini 3 Flash (preview), Gemini 3.5 Flash, Gemini 2.5 Pro. Note: several Gemini + older models were **removed from Copilot-on-web** May 2026 but remain on IDE surfaces. (source: https://github.blog/changelog/2026-05-20-updates-to-available-models-in-copilot-on-web/)
- **Other:** MAI-Code-1-Flash (Microsoft), Raptor mini, Kimi K2.7 Code.

### Model gating and multipliers
- **Premium-request multipliers are effectively deprecated** — post-2026-06-01 all chat/agent models are **token/credit-billed**, not multiplier-billed. Legacy "0.33× / 1× / 10×" framing is gone; the pricing table in the economics card is authoritative. (as of 2026-07-14, source: https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- **Code completions + next-edit suggestions are not credit-billed** — unlimited on paid plans.
- **Sol requires Pro+/Business/Enterprise** (not base Pro); Terra/Luna on all paid plans.
- **On Business/Enterprise the GPT-5.6 model policy is OFF by default** — an admin must enable it or nobody sees Sol/Terra/Luna. (as of 2026-07-09, source: https://github.blog/changelog/2026-07-09-openais-gpt-5-6-sol-terra-and-luna-are-now-available-in-github-copilot/)
- Reasoning-effort dial (VS Code / CLI): see economics card — not duplicated here.

## Admin / governance

**Policy settings (org, cascade from enterprise):** (as of 2026-07-14, source: https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies)
- **Model enablement** — per-model/family availability toggle (some models "may incur additional cost"). Enterprise policy overrides org; if the enterprise owner pins a policy, the org **cannot** override it.
- **Copilot in GitHub.com**, **MCP servers in Copilot** (GA-surface toggle only — does not gate the GitHub MCP server in third-party hosts), **third-party coding agents** (Anthropic Claude / OpenAI Codex).
- **Feedback + preview-feature opt-in** (Business/Enterprise) — keep preview features governed; treat "preview models with data retention" as OFF by default posture.
- **Public-code / duplication-detection filter** — blocks suggestions matching ~150 chars of public GitHub code. **Off by default on Business; enforced at policy level on Enterprise.** Must be ON to keep IP indemnity valid (below). (as of 2026-07-14, source: https://copilot.github.trust.page/faq)

**Content exclusions:** (as of 2026-07-14, source: https://docs.github.com/en/copilot/concepts/context/content-exclusion)
- Configurable at **repo / org / enterprise** level (Business/Enterprise only); REST API available.
- Supported in **VS Code, Visual Studio, JetBrains, neovim** completions/chat.
- **Big gap: NOT enforced in Copilot CLI, the cloud coding agent, or Agent/Edit modes of Copilot Chat.** Do not rely on content exclusion to keep secrets out of agentic surfaces — use repo hygiene instead.

**Audit / reporting:** (as of 2026-07-14, sources: https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/review-audit-logs · https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/review-activity/review-user-activity-data)
- Audit-log events (seat assign/unassign, policy changes) retained **180 days**; usage data **90 days (Business) / 365 days (Enterprise)**.
- **CSV usage reports** (active users, accepted suggestions, per-seat) + Copilot **Usage Metrics API** (legacy Metrics API shut down 2026-04-02). Seat management via REST API. Audit logs can stream to external SIEM (enterprise).

**IP indemnity:** GitHub/Microsoft defend + cover judgments for copyright claims on Copilot-generated code for Business/Enterprise **only if the duplication-detection filter was ON when generated.** (as of 2026-07-14, source: https://copilot.github.trust.page/faq)

**Data handling:** (as of 2026-07-14, source: https://docs.github.com/en/copilot/reference/data-and-privacy/privacy-github-copilot)
- **No training on Business/Enterprise prompts, code, or suggestions** (DPA-backed). Free/Pro inputs *may* be used for model improvement from 2026-04-24 (opt-out) — not relevant to this org's paid seats.
- **Claude-in-Copilot inference is routed to Anthropic's infrastructure** (GitHub-managed, under GitHub's DPA/no-train commitments for Business/Enterprise, not a direct Anthropic Enterprise contract) — but GitHub's public docs do **not** publish the inference *region* for third-party models. This is the same "who's terms / where" question flagged for M365 Copilot in [enterprise-ai-lanes.md](../enterprise-ai-lanes.md); confirm in writing before treating Claude routes as in-boundary. Same open question for OpenAI/Google-served models.

## Agentic surfaces

- **Copilot coding agent (cloud):** GitHub-hosted, autonomous — runs in an **ephemeral GitHub Actions environment**, opens PRs. **All paid plans; Business/Enterprise admin must enable the policy first.** Consumes **Actions minutes + AI credits** (token-based, model-dependent); within included allowances no extra charge. Repo owners can opt individual repos out; supports custom instructions + MCP. (as of 2026-07-14, source: https://docs.github.com/en/copilot/concepts/coding-agent/coding-agent)
- **Agent mode in IDE** (VS Code/VS/JetBrains) — interactive multi-file agent; **bills all session tokens** (long runs cost regardless of prompt count — see economics card).
- **Copilot CLI** — terminal agent with `--effort` dial; no content exclusion.

## Knowledge / context

- **Copilot Spaces** (Enterprise + org-owned) — replaced legacy **Copilot knowledge bases (sunset 2025-11-01)**; legacy KBs migrate via "Convert to Space." Org spaces shareable with admin/editor/viewer roles; GitHub-file sources auto-refresh. **Enterprise admins can disable personal-space sharing.** (as of 2026-07-14, source: https://docs.github.com/en/copilot/concepts/context/spaces)
- **MCP support** — GA on IDE + coding agent; gated by the org **"MCP servers in Copilot"** policy. Server allow-listing is the governance lever for what tools agents can reach.

## What's still unverified
- **Inference region** for Claude/OpenAI/Gemini models served inside Copilot — GitHub docs don't publish it; the "under whose terms / where" question is confirmed only at the no-training/DPA level, not the geographic level. (blocks a clean Canadian-residency answer for Copilot the same way it does for M365 Copilot)
- **Overage rate per credit** beyond included pool — one aggregator claimed $0.04/credit; docs don't print the line item (see economics card flag).
- **Six-level reasoning dial for GPT-5.6** — changelog claims six, generic VS Code doc still lists none→high; not screenshot-confirmed (economics card flag).
- Whether the **duplication filter is truly "enforced/on" by default on Enterprise** vs merely policy-available — sources phrase this two ways; confirm in the org's own policy panel.
- Exact **preview-model data-retention** default state per current org policy — verify in the live admin console, don't assume.
