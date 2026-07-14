---
topic: ChatGPT Business/Enterprise — OpenAI's knowledge-work platform (tiers, admin, residency)
volatility: check-monthly
related: ../enterprise-ai-lanes.md, ../canada-residency-paths.md, ../model-pricing.md, ../coding-tools/codex.md
---

# ChatGPT Business / Enterprise

> Per-platform card for OpenAI's org-facing ChatGPT product. Lane analysis and single-vendor case: [../enterprise-ai-lanes.md](../enterprise-ai-lanes.md). Residency detail: [../canada-residency-paths.md](../canada-residency-paths.md). Model API rates: [../model-pricing.md](../model-pricing.md). Don't duplicate pricing tables — cross-reference.

## Tiers & pricing

- Workspace lineup is **Business / Enterprise / Edu** (+ Teachers). "ChatGPT Team" was renamed **ChatGPT Business** on 2025-08-29; "ChatGPT Work" is umbrella marketing, not a SKU. No further rename as of check. (as of 2026-07-14, source: https://help.openai.com/en/articles/8792828)
- **Business: $20/seat/mo annual ($25 monthly), min 2 seats, self-serve.** Runs GPT-5.5 by default; includes GPTs, Projects, Company Knowledge, Deep Research, ChatGPT Agent, 60+ connectors, SAML SSO, admin controls, no-training default, Codex, SOC 2 Type 2 / ISO 27001/27017/27018/27701. Since 2026-04-02 a cheaper **Codex-only seat type** exists. (as of 2026-07-14, sources: https://help.openai.com/en/articles/8792828, https://www.beam.cloud/blog/chatgpt-enterprise-pricing)
- **Enterprise: no list price** — reported ~**$60/seat/mo (range $45–75)**, ~**150-seat annual-prepaid floor ≈ $108k/yr**; large deals (5,000+ seats) trend toward ~$40/seat. Widely reported, unofficial. OpenAI also offers credits-based **"flexible pricing"** for Enterprise/Edu/Business. No firsthand account of a sub-150-seat deal found. (as of 2026-07-14, sources: https://www.beam.cloud/blog/chatgpt-enterprise-pricing, https://help.openai.com/en/articles/11487671)
- Enterprise adds over Business: SCIM, Enterprise Key Management (EKM), RBAC/custom roles, data residency (10 regions), Global Admin Console, Compliance API, custom legal terms. (as of 2026-07-14, source: https://www.beam.cloud/blog/chatgpt-enterprise-pricing)

## Billing model — the Apr-2026 shared credit pool

- On **2026-04-02** OpenAI moved Codex (and agentic/token-heavy consumption) to **usage-based workspace credits** aligned 1:1 to API token prices (**1 credit ≈ $0.04**), applied to Plus/Pro/Business/new Enterprise. Standard seats include a Codex/agent baseline; heavy use draws from a **shared org credit pool** billed on top of seats. (as of 2026-07-14, sources: https://www.eesel.ai/blog/codex-pricing, https://help.openai.com/en/articles/20001106-codex-rate-card)
- **Implication:** heavy coders and Workspace Agents silently drain the org pool. Spend caps + per-team limits are now a required admin skill — set before, not after. See [../coding-tools/codex.md](../coding-tools/codex.md).

## Admin & governance

- **Identity:** SAML SSO (Business+), SCIM provisioning, domain verification, IP allowlists, EKM (Enterprise). (as of 2026-07-14, source: https://help.openai.com/en/collections/11585137-sso-scim-and-user-management)
- **Compliance API:** Enterprise workspace owners get archiving, audit trails, data redaction/retention, DLP, policy enforcement — direct or via third-party integrations. (as of 2026-07-14, source: https://intuitionlabs.ai/articles/chatgpt-enterprise-admin-controls-security)
- **Retention:** no training on business data (default all business plans); **30-day retention default, configurable**; zero-retention available on the API path. (as of 2026-07-14, source: https://openai.com/business-data/)
- **Admin console:** 2026 **Global Admin Console** (admin.openai.com) = tenant-level control plane over multiple workspaces — custom roles, enable/disable apps & actions, export usage/compliance logs. (as of 2026-07-14, source: https://intuitionlabs.ai/articles/chatgpt-enterprise-admin-controls-security)
- **Connectors/agents:**
  - **Workspace Agents** — successor to custom GPTs; shared team agents that run long workflows within org permissions, plug into Slack/Salesforce/etc. Research preview on Business/Enterprise/Edu/Teachers; free period extended to **2026-07-06**. Enterprise/Edu admins gate them by **role-based controls**; since **2026-05-07** supported on EKM workspaces; Business/Enterprise/Edu admins see agent activity/usage. (as of 2026-07-14, sources: https://openai.com/index/introducing-workspace-agents-in-chatgpt/, https://help.openai.com/en/articles/20001143-chatgpt-workspace-agents-for-enterprise-and-business)
  - **AgentKit** — developer/enterprise agent-build toolkit: **Agent Builder** (visual multi-agent canvas + versioning) and **Connector Registry** (single admin panel consolidating data sources across ChatGPT and the API). (as of 2026-07-14, source: https://openai.com/index/introducing-agentkit/)

## Data handling & residency (cross-ref — do not re-derive)

Settled in [../canada-residency-paths.md §3.2](../canada-residency-paths.md):
- **Canada is a live at-rest residency region** (storage only: conversations, files, GPTs, backups in-region). **Inference defaults to US.** Included at no extra cost. (as of 2026-07-14)
- Residency qualifies on **Enterprise/Edu/Healthcare/API only — Business does NOT** — so residency effectively requires the Enterprise contract, and a **new in-region workspace** (no migration). (as of 2026-07-14)
- In-region **inference** (GPU) exists for **US/Europe only — not Canada** (and isn't needed for the "at-rest CA / inference US" target). **ZDR is an API-path feature, not the ChatGPT app.** (as of 2026-07-14)

## Rollout / enablement surface

- **Admin console** (admin.openai.com) — workspaces, roles, app/action toggles, compliance log export.
- **Analytics** — usage/adoption reporting in-console; Compliance API feeds SIEM/DLP tooling.
- **Training** — OpenAI Academy (incl. Workspace Agents course) + Help Center release notes; product churns fast, so version internal material. (as of 2026-07-14, source: https://openai.com/academy/workspace-agents/)

## What's still unverified

- Exact Enterprise per-seat price + whether the ~150-seat floor bends via "flexible pricing" (sales-gated; no firsthand sub-150 datapoint found).
- CAD list pricing (all figures USD).
- Workspace Agents pricing after the 2026-07-06 free-period end.
- Which Codex-cloud artifacts store in-region for a Canada workspace (get in writing — see codex.md).
- Whether Business default model advances past GPT-5.5 to a 5.6-family default.
