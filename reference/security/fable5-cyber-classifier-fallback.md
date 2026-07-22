---
topic: What cyber work stays on Fable 5 vs routes to Opus 4.8, how the classifier decides, and why CVP runs on Opus/Sonnet
volatility: check-monthly
related: anthropic-cyber-verification-program.md, 00-ai-pentest-access.md, ../model-families.md
---

# Fable 5 cyber classifier: what stays, what routes to Opus

> **Status (2026-07-22): confirmed against Anthropic-owned primary sources** (anthropic.com, support.claude.com, code.claude.com) and the claude-code repo. Supersedes the "verify Fable-5 coverage at apply time" flag in [anthropic-cyber-verification-program.md](anthropic-cyber-verification-program.md).

## The mechanic

- Fable 5 ships dual-use cyber safeguards **on**. A four-tier classifier flags high-risk cyber content and the response is **re-run on the provider's default Opus 4.8** — session continues on Opus. Offensive-security / pentest / CTF workloads "trigger fallback frequently, often on the first request... This is expected routing for these domains, **not an account flag**." (as of 2026-07-22, source: https://code.claude.com/docs/en/model-config)
- Fable 5 and Mythos 5 are the **same underlying model**; Mythos has the safeguards lifted (Project Glasswing, invite-only). (as of 2026-07-22, source: https://www.anthropic.com/glasswing)

## Blocked by default on Fable (high-risk dual-use) — verbatim categories

- "Hacking, penetration testing, red teaming, and bug bounties"
- "Gaining cyber access through unexpected or unauthorized means: exploitation, credential attacks (brute force, spraying, stuffing, theft), and authentication bypasses"
- "Privilege escalation, lateral movement, and persistence"
- "Exploit development and weaponization (including zero-click and memory-corruption work)"
- "Virtual machine or container escapes"
- Security assessments targeting **ICS/SCADA, telecom core, and financial infrastructure**
- "High-uplift vulnerability finding"
- (as of 2026-07-22, source: https://www.anthropic.com/news/fable-safeguards-jailbreak-framework)

## Permitted — stays on Fable (verbatim)

- "Secure coding, and fixing simple or already identified vulnerabilities in code"
- "Debugging"
- "Log analysis, SOC analysis/enrichment, threat hunting, and incident response"
- "Patch management and deployment"
- "Malware reverse engineering"
- (as of 2026-07-22, source: https://www.anthropic.com/news/fable-safeguards-jailbreak-framework)

## Saying you're authorized does not keep it on Fable

- The classifier flags on **topic/content, not a self-declared authorization string**. Anthropic frames the distinguishing factor as "context — who is doing the work and under what authorization," and for Fable 5 says it will "block these types of actions until we have better controls to limit access to known good actors." The gate is **org-level verification, not prompt text**. (as of 2026-07-22, source: https://www.anthropic.com/news/fable-safeguards-jailbreak-framework)

## The classifier reads more than the typed prompt

- Checks "review everything the model reads, not just your latest message — including **memory, content from connectors, web search results, and files**, so a block can be triggered by content you didn't type." In Claude Code, the first request also carries **CLAUDE.md content and git status**; a repo containing security material "can trip the classifier on that context alone." (as of 2026-07-22, sources: https://support.claude.com/en/articles/15363606-why-claude-switched-models-in-your-conversation-with-fable-5 · https://code.claude.com/docs/en/model-config)
- Test whether workspace context is the trigger with **`claude --safe-mode`** (env `CLAUDE_CODE_SAFE_MODE`; landed in Claude Code CLI 2.1.169). It disables CLAUDE.md, skills, MCP servers, and hooks — but **git status and directory names are not customizations and are still included**. (as of 2026-07-22, source: https://code.claude.com/docs/en/model-config)

## CVP covers Opus/Sonnet only — not Fable

- The safeguards support article scopes itself: **"This article applies only to Opus and Sonnet class models."** CVP adjusts safeguards on Opus- and Sonnet-class models; **Fable 5 is not on the covered list.** Fable-class offensive capability is a separate trusted-access path (Project Glasswing / Mythos — invite-only), not CVP. (as of 2026-07-22, sources: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet · https://code.claude.com/docs/en/model-config · https://www.anthropic.com/glasswing)

## Open question — do NOT rely on it

- Whether Fable's **automatic fallback to Opus 4.8 inherits the caller's CVP-adjusted Opus safeguards**, or hits default Opus safeguards, is **undocumented**. A CVP-enrolled org's filed issue reports the routing layer flags content the org's CVP use-case safeguards already permit, and the work "completes on Opus" — but no source confirms which safeguard profile the fallback response runs under. Treat as unverified; confirm with an Anthropic account rep if it matters. (as of 2026-07-22, source: https://github.com/anthropics/claude-code/issues/67305)

Implication: for authorized offensive-security work under CVP, **call Opus 4.8 / Sonnet 5 directly** rather than relying on Fable's auto-fallback to carry CVP relief. The practical setup is all three models provisioned — Fable for defensive/everyday work (secure coding, SOC, malware RE), Opus/Sonnet under CVP for the flagged offensive work. See [anthropic-cyber-verification-program.md](anthropic-cyber-verification-program.md) for the apply flow and [00-ai-pentest-access.md](00-ai-pentest-access.md) for the channel overview.
