---
topic: Claude Code — enterprise licensing, deployment config, and governance
volatility: check-monthly
related: platforms/claude-enterprise-cowork.md, canada-residency-paths.md, alberta-velocity-program.md
---

# Claude Code (enterprise card)

> Tool-level card. Seat/plan pricing and the knowledge-work surface live in [claude-enterprise-cowork.md](../platforms/claude-enterprise-cowork.md). Residency mechanics live in [canada-residency-paths.md](../canada-residency-paths.md) §2 — cross-referenced, not re-derived.

## Licensing paths

- **Team Premium seat** — self-serve, includes Claude Code + Claude on web, centralized billing. **$100/seat/mo annual** per pricing page; **Claude Code docs table says $150 (Premium)** — discrepancy live, confirm with sales. (as of 2026-07-14, sources: https://claude.com/pricing, https://code.claude.com/docs/en/third-party-integrations)
- **Enterprise** — ~$20/seat + usage metered at API rates; adds SSO + domain capture, RBAC, Compliance API, server-managed policy settings for org-wide config. Min 20 self-serve / 50 sales-assisted. (as of 2026-07-14, source: https://support.claude.com/en/articles/9797531)
- **API key (Anthropic Console)** — PAYG, individual developers; no Claude-on-web bundle, no subscription-only features.
- **Cloud-provider backends** — PAYG through Amazon Bedrock, Claude Platform on AWS, Google Cloud's Agent Platform (Vertex), or Microsoft Foundry; billed via that cloud, not Anthropic. (as of 2026-07-14, source: https://code.claude.com/docs/en/third-party-integrations)

## Surfaces

CLI + Agent SDK, VS Code + JetBrains extensions, GitHub Actions / GitLab CI/CD — work on **every** provider. Claude Code **on the web, mobile, Desktop, and in Slack**, plus Routines (`/schedule`), Code Review, Remote Control — **require a claude.ai subscription** (unreachable via Console API key or a 3P cloud backend). (as of 2026-07-14, source: https://code.claude.com/docs/en/feature-availability)

## Enterprise config

- **Managed / server-managed settings:** a local **managed settings file** works on every provider; **server-managed settings** (policy pushed from the admin console, taking precedence over local config, non-overridable) are **Team + Enterprise only** and do NOT work on Bedrock/Vertex/Foundry backends. Managed policy controls allowed tools, commands, MCP servers, and network destinations. (as of 2026-07-14, source: https://code.claude.com/docs/en/feature-availability)
- **Permission modes:** `default` (prompt on first use), `acceptEdits`, `plan` (read-only), `bypassPermissions`; `forceLoginOrgUUID` pins org membership. (as of 2026-07-14, source: https://code.claude.com/docs/en/admin-setup)
- **Backend overrides (env vars):** `CLAUDE_CODE_USE_BEDROCK=1` + `AWS_REGION`; `CLAUDE_CODE_USE_FOUNDRY=1` + `ANTHROPIC_FOUNDRY_RESOURCE`; `CLAUDE_CODE_USE_VERTEX=1` + `CLOUD_ML_REGION` + `ANTHROPIC_VERTEX_PROJECT_ID`. LLM-gateway/proxy via `ANTHROPIC_*_BASE_URL` + `HTTPS_PROXY`. **Pin models** on cloud backends with `ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU,FABLE}_MODEL` or aliases lag the newest release. (as of 2026-07-14, source: https://code.claude.com/docs/en/third-party-integrations)

### What degrades on Bedrock / Vertex / Foundry backends

The CLI and everything local (subagents, hooks, skills, MCP, CLAUDE.md, checkpoints, sandboxing, OpenTelemetry metrics) is identical everywhere. What's **lost vs a Claude subscription** on these backends: all subscription-only surfaces above, plus **fast mode, Advisor, Channels, the analytics dashboard, and server-managed settings**. Provider specifics (as of 2026-07-14, source: https://code.claude.com/docs/en/feature-availability):

- **Bedrock:** also loses **web search** (use WebFetch w/ explicit URL); prompt-caching support varies by model.
- **Vertex:** web search only on Claude 4+ models.
- **Foundry:** also loses **GitHub Actions / GitLab CI/CD**.
- **All three:** auto mode limited to Sonnet 5 / Opus 4.7 / 4.8; `/loop` explicit intervals only (no self-pacing, no `/schedule`); ZDR subject to your cloud agreement. GitHub Actions/GitLab remain the cloud-session substitute (except Foundry).
- Also lost vs seats (from corpus): Claude-on-web bundle, Anthropic seat/usage dashboard (costs move to AWS Cost Explorer / GCP Billing / Azure Cost Mgmt), and server-side platform features — web search/fetch, code execution, Files API, Batches, automatic top-level prompt caching. Manual caching, thinking, tool use, 1M context still work. (as of 2026-07-14, source: ../canada-residency-paths.md §2.3)

## Governance

- **Audit logging:** **Compliance API** (Enterprise) streams activity-feed events, chat data, and file content to a SIEM — launched 2025-08-20; retain 90+ days for SOC 2. SOC 2 Type II + ISO 27001 certified. (as of 2026-07-14, sources: https://claude.com/blog/claude-platform-compliance-api, https://support.claude.com/en/articles/13015708)
- **Spend controls:** org- and user-level spend limits (Enterprise); Analytics **dashboard** on Team+Enterprise, Analytics **API** Enterprise-only. On cloud backends, budgets shift to the cloud's cost tooling + an LLM gateway. (as of 2026-07-14, source: https://code.claude.com/docs/en/feature-availability)
- **MCP allowlisting:** central team checks a `.mcp.json` into repos; managed settings restrict which MCP servers/tools/network destinations Claude can reach — non-overridable by developers. (as of 2026-07-14, source: https://code.claude.com/docs/en/third-party-integrations)
- **Subagents & hooks as admin surface:** org-wide CLAUDE.md deployable to system dirs (e.g. `/Library/Application Support/ClaudeCode/CLAUDE.md`); hooks enforce pre/post-action policy; subagents scoped by managed config. (as of 2026-07-14, source: https://code.claude.com/docs/en/third-party-integrations)

## Alberta deployment as config precedent (cross-reference)

Alberta (Crown gov) runs Claude Code at scale on **Bedrock ca-central-1 with US-geo cross-region inference** — `CLAUDE_CODE_USE_BEDROCK=1`, `AWS_REGION=ca-central-1`, model pinned to `us.anthropic.claude-opus-4-8`. Data at rest (logs, config, KBs) in Canada; inference in US on AWS's backbone; CloudTrail logs `inferenceRegion` per request; SCPs pin destination regions. This is the settled reference pattern for our profile — full detail in [alberta-velocity-program.md](../alberta-velocity-program.md) and [canada-residency-paths.md](../canada-residency-paths.md) §2.3. (as of 2026-07-14)

## What's still unverified

1. **Team Premium $100 vs $150** — pricing page vs Claude Code docs disagree; sales confirmation required.
2. Whether Sonnet 5 intro pricing applies on the Bedrock backend (corpus open item).
3. Claude model availability in **Foundry Canada East** — would enable `CLAUDE_CODE_USE_FOUNDRY` under the existing Azure agreement (portal check).
4. CAD list pricing (all figures USD).
