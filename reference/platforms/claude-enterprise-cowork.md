---
topic: Anthropic knowledge-work offering — Claude Team/Enterprise tiers + Claude Cowork
volatility: check-monthly
related: coding-tools/claude-code.md, canada-residency-paths.md, enterprise-ai-lanes.md
---

# Claude Enterprise & Cowork (knowledge-work lane)

> Per-platform card for Anthropic's non-coding knowledge-work offering. Coding-tool detail lives in [claude-code.md](../coding-tools/claude-code.md). Residency is cross-referenced, not re-derived — see [canada-residency-paths.md](../canada-residency-paths.md) §2.

## Tiers, minimums, pricing (all USD list)

- **Team Standard:** $20/seat/mo annual ($25 monthly), teams of 5–150, self-serve. All Claude features + more usage than Pro; SSO. **No Claude Code.** (as of 2026-07-14, source: https://claude.com/pricing)
- **Team Premium:** **$100/seat/mo annual ($125 monthly)** per the pricing page — 5× standard-seat usage, includes Claude Code + Cowork, PAYG top-ups, mix-and-match seat types in one team. (as of 2026-07-14, source: https://claude.com/pricing)
  - **OPEN — pricing discrepancy still live:** the Claude Code enterprise docs table lists Team Premium at **"$150/seat (Premium)"**, not $100. Both pages checked same day and disagree. Confirm current figure with sales before quoting. (as of 2026-07-14, sources: https://claude.com/pricing vs https://code.claude.com/docs/en/third-party-integrations)
- **Enterprise:** **~$20/seat platform fee (annual) + ALL usage metered at standard API rates** — no bundled/included token allowance since the Nov 2025–Feb 2026 billing change. Seat = access only. Min **20 seats self-serve / 50 sales-assisted**. Includes Claude Code + Cowork, SSO + domain capture, SCIM, audit logs, Compliance API, custom data retention, customer-managed encryption keys (CMEK), US-only inference option, HIPAA-ready config, org- and user-level spend limits. (as of 2026-07-14, sources: https://support.claude.com/en/articles/9797531, https://claude.com/pricing)

Cost implication at 1,000-knowledge-worker scale: Enterprise's metered-token model makes org-wide spend hard to forecast — the whole reason [enterprise-ai-lanes.md](../enterprise-ai-lanes.md) routes knowledge workers to Microsoft and reserves Anthropic for the ~15-coder lane.

## Claude Cowork

- **What it is:** Anthropic's agentic knowledge-work surface (the "Claude Code for the rest of the office") — runs multi-step tasks over connected files/apps. GA **2026-04-09** on all paid plans, desktop macOS/Windows. (as of 2026-07-14, source: https://claude.com/blog/cowork-for-enterprise)
- **Availability:** cloud/web/mobile version in **beta rollout since 2026-07-07**. (as of 2026-07-14, source: https://techcrunch.com/2026/07/07/the-coding-agent-wars-are-spilling-into-the-rest-of-the-office-claude-cowork/)
- **Licensing:** no separate charge — bundled on paid plans; usage metered like everything else on Enterprise. (as of 2026-07-14, source: https://claude.com/blog/cowork-for-enterprise)
- **Backend — SETTLED:** Cowork **cannot** be pointed at Bedrock/Vertex/Foundry. Unlike Claude Code there is no backend override; the feature request was closed **"not planned."** (as of 2026-07-14, source: https://github.com/anthropics/claude-code/issues/40526)
- **Data handling:** *local (desktop) sessions* — agent loop + code execution run on-device in a local VM, data stays on device/connected folders, but history is local-only (no central admin retention/export). *Remote/cloud sessions* (the beta) — per-session sandboxes on **Anthropic-managed US infrastructure**; files + conversation data stored on Anthropic servers, no published geographic commitment (presume US). (as of 2026-07-14, source: https://support.claude.com/en/articles/14479288)

## Admin / governance

- **Identity:** SSO + domain capture on Enterprise (SSO also on Team); **SCIM provisioning is Enterprise-only**. (as of 2026-07-14, source: https://support.claude.com/en/articles/9797531)
- **Audit / compliance:** **Compliance API** (Enterprise) pulls activity-feed events, chat data, and file content across all Claude deployments for SIEM ingestion — launched 2025-08-20. Cowork enterprise controls also stream OpenTelemetry to SIEM. (as of 2026-07-14, sources: https://claude.com/blog/claude-platform-compliance-api, https://support.claude.com/en/articles/13015708)
- **Cowork enterprise controls (Apr 2026):** RBAC/SCIM groups, per-team enablement, group spend limits, analytics dashboard + API, OpenTelemetry SIEM streaming, **per-tool MCP connector controls**, MDM-delivered keys. (as of 2026-07-14, source: https://claude.com/blog/cowork-for-enterprise)
- **Retention:** custom retention on Enterprise; ZDR available as a separate enablement for qualified Enterprise accounts (not standard) — **note Fable 5 requires 30-day retention and is excluded under ZDR**. (as of 2026-07-14, source: https://platform.claude.com/docs/en/manage-claude/data-residency)
- **Training:** Anthropic commercial (Team/Enterprise/API) does not train on customer data by default; Enterprise page emphasizes data-security controls + CMEK rather than restating the no-train default. (as of 2026-07-14, source: https://support.claude.com/en/articles/9797531)

## Residency (cross-reference — do not re-derive)

- First-party Anthropic workspace geo supports **only `us`** (immutable at creation); **no Canada option anywhere first-party.** Regional deployment via Bedrock/Vertex/Foundry is live for US/EU/APAC; **Canada = "Coming 2026" on all three**. There is **no in-region Claude inference in Canada** — flagship Claude on Bedrock ca-central-1 is callable only via cross-region (US-geo/global) profiles. Full detail + the Cowork residency gap: [canada-residency-paths.md](../canada-residency-paths.md) §2. (as of 2026-07-14)
- Net for Cowork specifically: **no Cowork-with-Canadian-data-at-rest exists on any channel today** — desktop-local sessions (data on managed devices) are the only interim, or wait for Anthropic Canada region GA.

## What's still unverified

1. **Team Premium $100 (pricing page) vs $150 (Claude Code docs)** — unresolved from public pages; sales confirmation required.
2. Whether the Cowork cloud/web beta will ever gain a Bedrock/Vertex/Foundry backend (currently "not planned") or publish a storage-region commitment.
3. CAD list pricing for all tiers (all figures above are USD).
4. Exact no-training contractual language for Cowork cloud sessions (inferred from platform default; not restated on the Enterprise page).
