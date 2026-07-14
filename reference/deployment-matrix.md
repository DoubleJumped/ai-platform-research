---
topic: Consolidated model × surface × config deployment matrix — "can I run model X on surface Y in config Z?"
volatility: check-monthly
related: canada-residency-paths.md, canadian-data-requirements.md, model-families.md, model-pricing.md, platforms/microsoft-stack.md, platforms/chatgpt-enterprise.md, platforms/claude-enterprise-cowork.md, coding-tools/github-copilot.md, coding-tools/claude-code.md, coding-tools/codex.md, 5.6-use-guide/00-guide.md
---

# Deployment matrix

> Status: synthesis card, not a vendor decision. No path here is the settled pick (see CLAUDE.md rule 6).

## How to read this

Cells: ✅ available · ❌ not offered on this surface · ⚠️ available-with-caveat (see numbered footnote). Every caveat is stamped and cites the corpus file it came from — this file does not re-derive facts. Where two corpus files disagreed, the **newer stamp won** and it is noted at the footnote. The matrix is split into chat/knowledge surfaces (Table A) and coding/API surfaces (Table B) so each stays readable in raw markdown. Config dimensions (residency, ZDR, dials, spend) are in the third table, keyed by surface.

## Table A — chat / knowledge surfaces

| Model | ChatGPT app | M365 Copilot | Copilot Cowork (MS) | Claude apps | Claude Cowork |
|---|---|---|---|---|---|
| GPT-5.6 Sol / Terra / Luna | ✅ | ⚠️1 | ⚠️2 | ❌ | ❌ |
| GPT-5.5 | ✅ | ✅ | ✅ | ❌ | ❌ |
| GPT-5.4 (+ mini/nano) | ✅ | ⚠️1 | ❌ | ❌ | ❌ |
| Claude Opus 4.8 | ❌ | ✅3 | ✅ | ✅ | ✅ |
| Claude Sonnet 5 | ❌ | ⚠️4 | ❌4 | ✅ | ✅ |
| Claude Sonnet 4.6 | ❌ | ✅3 | ✅ | ✅ | ✅ |
| Claude Fable 5 | ❌ | ⚠️5 | ⚠️5 | ✅ | ✅6 |
| Claude Haiku 4.5 | ❌ | ❌ | ❌ | ✅ | ✅ |
| Gemini / Grok | ❌ | ❌ | ❌ | ❌ | ❌ |

## Table B — coding / API surfaces

| Model | Codex | GitHub Copilot | Claude Code | Bedrock | Vertex | Foundry | Direct API |
|---|---|---|---|---|---|---|---|
| GPT-5.6 Sol / Terra / Luna | ✅7 | ✅8 | ❌ | ❌9 | ❌9 | ✅10 | ✅ |
| GPT-5.5 | ✅ | ✅ | ❌ | ❌9 | ❌9 | ✅10 | ✅ |
| GPT-5.4 (+ mini/nano) | ✅ | ✅ | ❌ | ❌9 | ❌9 | ✅10 | ✅ |
| gpt-5.3-codex | ⚠️11 | ✅ | ❌ | ❌9 | ❌9 | ✅10 | ✅ |
| Claude Fable 5 | ❌ | ✅ | ✅ | ✅12 | ✅13 | ⚠️14 | ✅ |
| Claude Opus 4.8 | ❌ | ✅ | ✅ | ✅12 | ✅13 | ⚠️14 | ✅ |
| Claude Sonnet 5 | ❌ | ✅ | ✅ | ✅12,15 | ✅13 | ⚠️14 | ✅ |
| Claude Sonnet 4.6 | ❌ | ✅ | ✅ | ✅12 | ✅13 | ⚠️14 | ✅ |
| Claude Haiku 4.5 | ❌ | ✅ | ✅ | ✅12 | ✅13 | ⚠️14 | ✅ |
| Gemini 3.x | ❌ | ✅16 | ❌ | ❌ | ✅17 | ❌ | ✅ |

## Config dimensions (per surface)

| Surface | Canada data-at-rest | Inference location | ZDR | No-training default | Effort/reasoning dial | Spend control |
|---|---|---|---|---|---|---|
| ChatGPT app | ⚠️ Enterprise/Edu only, new in-region workspace; Business ❌ [a] | US (default) | ❌ (API path only) | ✅ all business plans | model pick; no per-task effort dial in chat | shared org credit pool, per-team caps, Global Admin Console |
| Codex | ⚠️ honors workspace residency; cloud-artifact scope unverified [b] | US | via `ca.api.openai.com` API path only | ✅ | `--effort` (CLI) | shared org credit pool (1 cr ≈ $0.04), per-team caps |
| GitHub Copilot | ❌ inference region not published [c] | provider infra, region not published | n/a (no-train DPA) | ✅ Business/Enterprise | reasoning-effort dial (VS Code/CLI); 6-level for 5.6 unverified | AI credits pool, org/enterprise policy cascade |
| M365 Copilot / MS Cowork | ✅ tenant residency now; Anthropic routes excluded; in-country CA 2027 [d] | global; in-country CA processing announced 2027 | n/a (M365 trust boundary + Purview) | ✅ | model pick; Cowork light/med/heavy tiers | Copilot Credits ($0.01), tenant/group/user spend limits |
| Claude apps / Cowork | ❌ US-only first-party ("coming 2026"); Cowork cloud = US, desktop-local = on-device [e] | US / global | Enterprise enablement, excludes Fable 5 | ✅ commercial default | extended-thinking toggle | Enterprise org/user spend limits; usage metered at API rates |
| Claude Code | ⚠️ via Bedrock ca-central-1 (Alberta pattern) or Foundry canadaeast (unverified); seats = US [f] | US-geo cross-region profile; no in-region CA | subject to cloud agreement; excludes Fable 5 | ✅ | thinking; auto mode limited to Sonnet 5/Opus 4.7-4.8 on backends | cloud cost tools + LLM gateway on backends; spend limits on seats |
| Bedrock | ✅ ca-central-1 (logs, KBs, config) | cross-region US-geo/global; no in-region CA Claude | per AWS agreement | ✅ AWS is processor; Anthropic never sees data | API `thinking` param | AWS Cost Explorer; SCP region pinning; ~10% geo premium |
| Vertex | ❌ no `ca` region for Claude (US/EU only) | US / EU | per GCP agreement | ✅ | API param | GCP Billing |
| Foundry (Azure) | ⚠️ canadaeast Global Standard = Canada geography; confirm not "Americas" [g] | global (incl. US); regional PTU tops at gpt-5.2 | per Azure agreement | ✅ | deployment/API param | Azure Cost Mgmt; DataZone ≈ +10%, Batch 50% off |
| Direct API | ⚠️ OpenAI `ca.api.openai.com` storage-only CA; Anthropic US-only [h] | US (OpenAI proc. US/EU/UAE); Anthropic US/global | ✅ both (Anthropic excludes Fable 5; OpenAI ZDR amendment) | ✅ | full reasoning/effort params | usage dashboards, per-project limits |

## Footnotes (matrix cells)

1. M365 Copilot roster = "OpenAI GPT-5.x (Azure-hosted)"; card names GPT-5.5 explicitly but does not individually confirm Sol/Terra/Luna or 5.4 in the Copilot picker. (as of 2026-07-14, source: platforms/microsoft-stack.md §2)
2. Microsoft Copilot Cowork GA ships GPT-5.5 (Frontier program); GPT-5.6 not listed in the GA roster. (as of 2026-07-14, source: platforms/microsoft-stack.md §1)
3. Anthropic-routed Copilot/Cowork traffic on by default in most commercial regions since 2026-01-07, but **excluded from the EU Data Boundary and in-country processing commitments** — the residency landmine. (as of 2026-07-14, source: platforms/microsoft-stack.md §2)
4. Microsoft Copilot/Cowork ship **Sonnet 4.6, not Sonnet 5**, in the current roster. (as of 2026-07-14, source: platforms/microsoft-stack.md §1–2)
5. Fable 5 = "Preview model with Data Retention," **default-OFF everywhere**; under it Anthropic is an independent processor (not MS subprocessor) retaining I/O up to 30 days. (as of 2026-07-14, source: platforms/microsoft-stack.md §2)
6. Fable 5 requires 30-day retention and is **excluded under ZDR**. (as of 2026-07-14, source: platforms/claude-enterprise-cowork.md; model-families.md)
7. Codex default coding models are now the GPT-5.6 family (Sol/Terra/Luna); credits per MTok Sol 125/750, Terra 62.5/375, Luna 25/150. (as of 2026-07-14, source: coding-tools/codex.md)
8. Sol requires Pro+/Business/Enterprise (not base Pro); **GPT-5.6 model policy is OFF by default** on Business/Enterprise — admin must enable. Terra/Luna on all paid plans. (as of 2026-07-14, source: coding-tools/github-copilot.md)
9. Bedrock and Vertex do not host OpenAI models — not a residency path for GPT; use Foundry canadaeast or `ca.api.openai.com` instead. (as of 2026-07-14, source: canada-residency-paths.md §3.5, §3.4)
10. Deployable Global Standard in **canadaeast** (region table updated 2026-07-08): 5.6 sol/terra/luna, 5.5, 5.4 family, 5.3-chat, 5.3-codex, 5.2 family. Global Standard = at-rest in Canada geography + inference globally. (as of 2026-07-14, source: canada-residency-paths.md §3.5)
11. `gpt-5.3-codex` is **no longer a listed Codex product model** (superseded by GPT-5.6 family + Codex-Spark preview); it persists only as an Azure Foundry deployment target. (as of 2026-07-14, source: coding-tools/codex.md — newer than model-pricing.md's standalone listing)
12. Bedrock: flagship Claude is **not ca-central-1-native** — callable from ca-central-1 only via cross-region (US-geo/global) inference profiles, so inference leaves Canada. Geo profile ≈ 10% premium. Opus 4.8 + Sonnet 5 confirmed day-one on Bedrock (settles the older "not on Bedrock" concern). (as of 2026-07-14, source: canada-residency-paths.md §2.3)
13. Vertex: Claude multi-region endpoints are **US and EU only — no `ca`/Montreal region**, and endpoint region = both storage and inference (no store-here/infer-there split). (as of 2026-07-14, source: canada-residency-paths.md §2.4)
14. Claude is GA in Microsoft Foundry (not deployable in EU), but **canadaeast availability is UNVERIFIED** — check the Foundry model catalog in-tenant before relying on `CLAUDE_CODE_USE_FOUNDRY`. (as of 2026-07-14, source: canada-residency-paths.md §2.5; coding-tools/claude-code.md)
15. Whether Sonnet 5 intro pricing ($2/$10 through 2026-08-31) applies on the Bedrock backend is unconfirmed. (as of 2026-07-14, source: canada-residency-paths.md §2.3)
16. Gemini in Copilot: 3.1 Pro (preview), 3 Flash (preview), 3.5 Flash, 2.5 Pro; several Gemini + older models **removed from Copilot-on-web May 2026** but remain on IDE surfaces. (as of 2026-07-14, source: coding-tools/github-copilot.md)
17. Gemini is native to Vertex (Google's own platform); listed for completeness, outside the Claude/OpenAI residency analysis. (as of 2026-07-14, source: general — not a corpus residency path)

## Config-table footnotes

- **[a]** Residency qualifies on Enterprise/Edu/Healthcare/API only; storage-at-rest in Canada, inference US; requires a new in-region workspace (no migration). (as of 2026-07-14, source: chatgpt-enterprise.md; canada-residency-paths.md §3.2)
- **[b]** Codex honors workspace residency (backend enforces region auth for non-US workspaces); which cloud artifacts (repos, containers, task logs) store in-region is not publicly documented — get in writing. (as of 2026-07-14, source: coding-tools/codex.md)
- **[c]** GitHub does not publish the inference region for third-party (Claude/OpenAI/Gemini) models served in Copilot — blocks a clean Canadian-residency answer; confirmed only at the no-training/DPA level. (as of 2026-07-14, source: coding-tools/github-copilot.md)
- **[d]** Copilot Cowork follows the same residency model as M365 Copilot; Anthropic-routed traffic excluded from in-country commitments (Canada in-country processing announced 2027). (as of 2026-07-14, source: platforms/microsoft-stack.md §1–2)
- **[e]** First-party Anthropic workspace geo supports only `us` (immutable); Cowork cannot be pointed at Bedrock/Vertex/Foundry ("not planned"). Desktop-local sessions keep data on-device but have no central admin retention. (as of 2026-07-14, source: canada-residency-paths.md §2.2, §2.6; claude-enterprise-cowork.md)
- **[f]** Server-managed settings (Team/Enterprise) do NOT work on Bedrock/Vertex/Foundry backends; a local managed-settings file does. (as of 2026-07-14, source: coding-tools/claude-code.md)
- **[g]** All Foundry deployment types keep at-rest in the designated Azure geography; get Microsoft's written confirmation that canadaeast at-rest = Canada geography, not "Americas." (as of 2026-07-14, source: canada-residency-paths.md §3.5)
- **[h]** `ca.api.openai.com` = Canada storage-only region (regional processing US/EU/UAE); Anthropic first-party has no Canada option anywhere. (as of 2026-07-14, source: canada-residency-paths.md §3.4, §2.2)

## Known gaps (unverified cells that affect the matrix)

- **Claude in Foundry canadaeast** — GA in Foundry, but canadaeast deployability unconfirmed (footnote 14). A 5-minute tenant portal check resolves it and would unlock the lowest-friction Claude coding lane under the existing Azure agreement.
- **Copilot inference geography** — GitHub doesn't publish where Claude/OpenAI/Gemini models run when served inside Copilot (footnote [c]); no clean residency answer until confirmed in writing.
- **Codex-cloud in-region artifact scope** for a Canada workspace — not publicly documented (footnote [b]); sales letter needed.
- **M365 Copilot roster granularity** — whether Sol/Terra/Luna and GPT-5.4 are individually selectable (footnotes 1–2); confirm in the tenant Copilot admin center.
- **Azure "Americas vs Canada geography"** wording for canadaeast at-rest (footnote [g]) — Microsoft written confirmation outstanding.
- **Sonnet 5 intro pricing on Bedrock** (footnote 15), and **Team Premium $100 vs $150** discrepancy (sales) — both open across the corpus.
