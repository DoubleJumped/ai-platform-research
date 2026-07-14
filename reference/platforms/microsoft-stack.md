---
topic: Microsoft knowledge-work AI stack (Copilot Cowork · M365 Copilot · Copilot Studio/Agent 365 · Foundry) as one lane
volatility: check-monthly
related: ../m365-copilot.md, ../enterprise-ai-lanes.md, ../canada-residency-paths.md
---

# Microsoft AI stack — platform card

One lane, four layers: **Copilot Cowork** (agentic knowledge work — our currently deployed product) → **M365 Copilot** (assistant + seats) → **Copilot Studio / Agent 365** (build & govern agents) → **Foundry** (API/hosting). All layers share one identity (Entra), one compliance surface (Purview), one tenant. Prices USD list.

## 1. Copilot Cowork — the deployed knowledge-work product

Microsoft's agentic system for complex, long-running, multi-tool tasks: you define the work, Cowork plans/reasons/acts across files and tools and returns a finished result. This is the Microsoft analogue to Claude Cowork — **not** the same product, and **not** the same as Copilot chat or Copilot Studio.

- **GA worldwide 2026-06-16.** Microsoft claims >half of Fortune 500 already using it. (as of 2026-07-14, source: https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/16/copilot-cowork-is-now-generally-available/)
- **Licensing:** requires the **Microsoft 365 Copilot USL** (the $30 seat) as a prerequisite — Cowork is an execution layer on top, not a standalone SKU. (as of 2026-07-14, source: same GA blog)
- **Pricing = usage/consumption, not per-seat.** Billed in **Copilot Credits**, priced from four inputs: model use, context retrieval, tool calls, runtime. PayGo **$0.01/credit**; a "P3" advance-commitment tier discounts that. Per-task cost shown to users; scales light/medium/heavy. (as of 2026-07-14, source: same GA blog)
- **Models (multi-model, user-pickable):** GA ships **Anthropic Claude Opus 4.8 + Sonnet 4.6**, a **Sonnet+Opus "Advisor" pairing**, **GPT-5.5** (Frontier program), **Imagen 2** for images; **Claude Fable 5 (Preview)** is available but **off by default**. A first-party **"Cowork 1"** model is promised for lower cost. (as of 2026-07-14, sources: https://learn.microsoft.com/en-us/microsoft-365/copilot/cowork/cowork-admin-governance · https://m365admin.handsontek.net/anthropic-claude-fable-5-available-copilot-cowork-frontier/)
- **Admin/governance:** disabled by default (admin enables usage-based billing per tenant); spend limits at **tenant/group/user**; usage alerts; user-initiated credit requests; discoverability toggle; per-plugin/connector control; **Cowork Browsing** tenant setting (local Edge, inherits Conditional Access + DLP); each browser task in the unified audit log. Anthropic family can be turned off in M365 admin center under Copilot settings. (as of 2026-07-14, source: cowork-admin-governance Learn page above)
- **Data handling:** operates inside the "**Microsoft 365 trust boundary**" — prompts/responses/artifacts flow through existing M365 controls, governed/discoverable/retained; sensitivity labels inherited; **Purview** (audit, DSPM, eDiscovery, Insider Risk, Communication Compliance) applies; DLP "coming soon." Cloud-hosted (files not stored locally). (as of 2026-07-14, source: GA blog + cowork-admin-governance)
- **Residency:** "Copilot Cowork follows the same data residency model as Copilot." So the M365-Copilot residency caveats carry over verbatim — including the Anthropic landmine below. (as of 2026-07-14, source: cowork-admin-governance Learn page)

Implication: Cowork is live in our org but its Anthropic-routed and Preview-with-retention paths inherit the same residency exclusions as §2 — govern them the same way.

## 2. M365 Copilot — seats, models, governance

- **Enterprise pricing:** $30/user/mo add-on on a qualifying base. All-in ≈ **$66–90/user/mo** depending on base: E3 **$39** (rose from $36, Jul 2026) or E5 **~$60** (rose from $57). (as of 2026-07-14, source: https://www.microsoft.com/en-us/microsoft-365-copilot/pricing/enterprise) — reconcile against standalone Copilot Business $18 promo in [../m365-copilot.md](../m365-copilot.md).
- **E7 "Frontier Suite":** **$99/user/mo** annual, launched 2026-05-01 = E5 + M365 Copilot + **Agent 365** + Entra Suite in one SKU. (as of 2026-07-14, source: https://www.epcgroup.net/microsoft-365-copilot-pricing-licensing-enterprise-guide-2026)
- **Model roster inside Copilot:** OpenAI GPT-5.x (Azure-hosted, "no data leaves Microsoft") + **Anthropic Claude** (Opus/Sonnet; Fable 5 as a Preview-with-retention model). **Researcher agent** uses Opus reasoning with Critique/Model Council modes (a second, often OpenAI, model reviews/judges). (as of 2026-07-14, sources: https://learn.microsoft.com/en-us/microsoft-365/copilot/ai-models-overview · https://learn.microsoft.com/en-us/microsoft-365/copilot/connect-to-ai-subprocessor)
- **Anthropic default state (verified):** on by default in most commercial regions since **2026-01-07**; **off by default in EU/EFTA/UK** (opt-in setting added 2026-04-03). "Preview models with Data Retention" (e.g. Fable 5) are **default-OFF everywhere**; under them Anthropic is an **independent data processor, not a Microsoft subprocessor**, retaining most I/O up to **30 days**. (as of 2026-07-14, source: connect-to-ai-subprocessor Learn page)
- **Landmine (still current):** Anthropic-routed Copilot/Cowork traffic is **excluded from the EU Data Boundary and from Microsoft's in-country processing commitments** (Canada in-country announced for **2027**, see [../canada-residency-paths.md](../canada-residency-paths.md) §3.5). Scope or disable via **Entra** groups in M365 admin center; keep **Preview-with-retention OFF** for sensitive data — those run under Anthropic's terms, not Microsoft's DPA. (as of 2026-07-14, sources: connect-to-ai-subprocessor · [../enterprise-ai-lanes.md](../enterprise-ai-lanes.md))
- **Governance surface:** Copilot Dashboard/adoption analytics, **Purview** (DSPM for AI, eDiscovery, audit, sensitivity labels, DLP), **Entra** per-user/per-group model scoping. Everything stays inside Entra identity + tenant audit/eDiscovery. (as of 2026-07-14, source: connect-to-ai-subprocessor)

## 3. Copilot Studio / Agent 365 — the agent-building layer

- **Copilot Studio pricing:** entry **$200/mo per tenant**; consumption in **credits** — **PAYG $0.01/credit** or **message packs $200 = 25,000 credits ($0.008/credit)**. Per-action credit cost: 1 scripted answer, 2 generative, 5 agent action, 10 tenant-graph grounding, ~100 per 10 reasoning-model responses. (as of 2026-07-14, sources: https://www.microsoft.com/en-us/microsoft-365-copilot/pricing/copilot-studio · https://learn.microsoft.com/en-us/microsoft-copilot-studio/billing-licensing · https://www.cloudzero.com/blog/copilot-studio-pricing/)
- **Agent 365:** Microsoft's agent-governance layer (registry/identity/security/observability for agents), bundled into the E7 SKU (§2). Detailed standalone pricing — unverified, check the E7/Agent 365 licensing pages.
- **When it's the right tool:** custom, reusable agents grounded on tenant data/connectors that must be governed centrally (vs. Cowork's ad-hoc, user-driven task execution). Watch the credit burn — agent actions + graph grounding + reasoning stack up fast at scale.

## 4. Foundry — the API/hosting layer

Foundry is the model/API + agent-hosting floor of this stack; it has **no ChatGPT-style end-user UI** — knowledge workers reach it only via custom agents (Foundry Agent Service, publishable into Teams/M365 Copilot) or via M365 Copilot itself. For our Canada-residency angle (Canada East, Global Standard = at-rest in Canada + global inference; GPT-5.x + gpt-5.3-codex deployable; Claude GA in Foundry but Canada East availability unverified), see [../canada-residency-paths.md](../canada-residency-paths.md) §2.5, §3.5 — not duplicated here.

## What's still unverified

- Cowork **"P3" advance-commitment** discount rate and the exact credit-cost of a typical task (light/medium/heavy) — check the Cowork cost estimator / licensing guide.
- **Cowork 1** first-party model — timing, price, quality — not yet shipped.
- **Agent 365** standalone (non-E7) pricing and its precise governance feature set.
- Whether Cowork's residency inheritance means Anthropic-routed Cowork tasks are likewise excluded from Canada in-country processing (near-certain given "same model as Copilot," but not separately documented) — get Microsoft written confirmation.
- E3 $39 / E5 $60 all-in figures are from a third-party guide; confirm against the Microsoft enterprise pricing page and reconcile with the $18 Business promo in [../m365-copilot.md](../m365-copilot.md).
- All figures USD list — CAD needed for any business case.
