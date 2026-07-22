---
topic: Anthropic Cyber Verification Program (CVP) — the self-apply channel for authorized pentest on Claude
volatility: check-monthly
related: 00-ai-pentest-access.md, authorization-and-compliance.md, ../model-families.md
---

# Anthropic: Cyber Verification Program (CVP)

The real, self-apply channel for a verified defender to get Claude's dual-use cyber safeguards **adjusted** for authorized offensive-security work. Not Mythos — see the bottom of this file for why.

## What it is

- **CVP adjusts the real-time cyber safeguards on GA Claude models for verified defenders** — unlocking vulnerability-exploitation analysis, offensive security tooling, and pentest/red-team workflows that the default safeguards refuse. Categorically prohibited uses (mass-exfiltration tooling, ransomware, C2) stay blocked regardless of verification. (as of 2026-07-21, source: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude)
- **CVP covers Opus- and Sonnet-class models only — Fable 5 is NOT covered.** The support article scopes itself: "This article applies only to Opus and Sonnet class models." Fable 5 blocks high-risk dual-use by default and auto-routes flagged prompts to Opus 4.8; run authorized offensive work on **Opus 4.8 / Sonnet 5 directly** under CVP, and do not rely on Fable's fallback to inherit CVP relief (undocumented). Full detail: [fable5-cyber-classifier-fallback.md](fable5-cyber-classifier-fallback.md). (as of 2026-07-22, source: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet)

## How to apply

- **Direct form (start here):** https://claude.com/form/cyber-use-case — "fill out this form to apply for safeguards adjustment under our Cyber Verification Program." Asks for name, org, work email, use-case category (e.g. **"Authorized Penetration Testing & Red Teaming"**), and a use-case description. (as of 2026-07-21, verified live)
- **First-party portal:** https://portal.anthropic.com/programs/cvp — "Apply for and manage trust programs for your Claude usage." Login gate (Google/email/SSO). (as of 2026-07-21, verified live)
- **Gate:** approval-required, **~2 business days** target for a decision. (as of 2026-07-21, source: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude)
- Related forms (cited inside the support article, not independently verified — treat as secondary): appeal a false-positive/rejection `https://claude.com/form/cyber-block-false-positive-report-cvp-rejection-appeal`; platform owners `https://claude.com/form/platform-cvp-interest`.

## Two hard caveats

- **Data retention must be ON.** Zero-Data-Retention (ZDR) customers are **ineligible for the self-serve form** and must route safeguard adjustments through their Anthropic Sales rep. This matters for a Crown corp that turned ZDR on for residency reasons. (as of 2026-07-21, source: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude)
- **Surface coverage is uneven.** CVP is available on **Claude.ai, Claude Code, the Anthropic API (incl. BYOK), and Microsoft Foundry/Azure** (choose "Azure" as the Surface in the form). CVP is **NOT available on Amazon Bedrock or Google Vertex AI** — a Bedrock ca-central-1 deployment would need a custom enterprise agreement instead. (as of 2026-07-21, source: https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude)

## Why not Mythos 5 / Project Glasswing

- **Mythos 5 = Fable 5 with the safeguards removed entirely**, but it is invitation-only. Eligibility bar: partners maintain critical infrastructure or widely-relied-upon codebases where "a major attack could affect more than 100 million people," must meet Anthropic's security requirements, and are **selected, not self-applied**. Ordinary enterprises are steered to CVP / the GA Claude Security product instead. (as of 2026-07-21, source: https://www.anthropic.com/news/expanding-project-glasswing)
- The only public Mythos surface is an interest/waitlist (not an application): https://claude.com/form/mythos-access-interest — a notify-me signup, no self-serve access. A systematic trusted-access program for cyber orgs is described as *planned*, not live. (as of 2026-07-21, source: https://www.anthropic.com/claude/mythos)
- **Frontier Red Team is NOT an access program** — it's Anthropic's internal research team; the only "apply" links are job postings. Do not treat it as a model-access channel. (as of 2026-07-21, source: https://www.anthropic.com/research/team/frontier-red-team)

## Adjacent: Claude Security (defensive, GA)

- For the defensive side (scan code, validate findings, propose patches), **Claude Security** is GA and is the recommended offering for orgs that don't qualify for Glasswing. Access via https://claude.com/solutions/cybersecurity → "Start defending" or contact-sales. (as of 2026-07-21, source: https://claude.com/solutions/cybersecurity)

Implication: for authorized pentest, the ask is **CVP via https://claude.com/form/cyber-use-case**, with a documented RoE ready and ZDR/surface caveats checked first. Not Mythos.

Note: `https://www.anthropic.com/legal/usage-policy` is a dead 404 — the canonical policy URL is `https://www.anthropic.com/legal/aup`.
