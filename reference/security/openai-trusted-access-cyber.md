---
topic: OpenAI Trusted Access for Cyber (TAC) — GPT-5.5-Cyber access for authorized pentest
volatility: check-monthly
related: 00-ai-pentest-access.md, authorization-and-compliance.md, ../model-families.md
---

# OpenAI: Trusted Access for Cyber (TAC) / Daybreak

"OpenAI's cyber model" = **GPT-5.5-Cyber**, obtained through the **Trusted Access for Cyber (TAC)** program under the **Daybreak** cybersecurity umbrella. This is the real thing the phrase refers to.

## The three access tiers (it's not one model)

- **GPT-5.5 (standard)** — public, standard safeguards. (as of 2026-07-21, source: https://openai.com/index/gpt-5-5-with-trusted-access-for-cyber/)
- **GPT-5.5 with Trusted Access for Cyber** — verified defenders get **fewer classifier refusals** on authorized defensive work (vuln triage, malware analysis, reverse engineering, detection engineering, patch validation). OpenAI calls this "the right starting point for most defenders." (as of 2026-07-21, source: https://openai.com/index/gpt-5-5-with-trusted-access-for-cyber/)
- **GPT-5.5-Cyber** — a separately fine-tuned, **more permissive** variant for red teaming, penetration testing, exploit validation/development, and controlled validation in authorized environments. Requires additional approval beyond baseline TAC; in **limited preview** as of 2026-07-21. (as of 2026-07-21, source: https://www.helpnetsecurity.com/2026/05/08/openai-gpt-5-5-cyber-model/)

Daybreak also bundles **Codex Security** (agentic security researcher — autonomous vuln discovery/validation/patching; reported to be the rebranded "Aardvark," press/social-sourced, not first-party-confirmed). So: Aardvark→Codex Security is the *agent*; GPT-5.5-Cyber is the *gated model*; both sit under Daybreak, gated by TAC. (as of 2026-07-21, sources: https://openai.com/daybreak/ · https://thehackernews.com/2026/05/openai-launches-daybreak-for-ai-powered.html)

Version note (volatile): the cyber lineage went GPT-5.4-Cyber (Apr 2026) → GPT-5.5-Cyber (May 2026, current). A newer GPT-5.6 "Sol" previewed Jun 2026 has **restricted, government-gated** access and stronger safeguards — not the general TAC path. For a Crown corp today, the relevant model is **GPT-5.5-Cyber via TAC**. (as of 2026-07-21, source: https://thehackernews.com/2026/06/openai-limits-gpt-56-rollout-as-sol.html)

## How to apply

- **Organizations (the correct path for a Crown corp):** https://openai.com/form/enterprise-trusted-access-for-cyber/ — "Request OpenAI Pilot: Trusted Access For Cyber." **Gate: enterprise application + manual review** (not self-serve; approval is not automatic — weighs identity/trust verification, risk, intended use, and org cybersecurity capability). Alternative: request through an existing OpenAI account rep. (as of 2026-07-21, source: https://help.openai.com/en/articles/20001258-openai-daybreak-trusted-access-for-cyber-overview)
- **Individuals (get named users ready in parallel):** https://chatgpt.com/cyber — KYC identity verification (government ID + trust signals), reviewable in minutes. From **2026-06-01**, individuals on GPT-5.5-Cyber via TAC must enable **Advanced Account Security** (phishing-resistant MFA). (as of 2026-07-21, source: https://help.openai.com/en/articles/20001258-openai-daybreak-trusted-access-for-cyber-overview)
- Eligibility framing targets **critical-infrastructure operators, security vendors, and researchers** — a Crown corp securing its own infrastructure fits the "critical-infrastructure operator" framing. (as of 2026-07-21, source: https://openai.com/index/gpt-5-5-with-trusted-access-for-cyber/)
- Troubleshooting reference: https://help.openai.com/en/articles/20001259-trusted-access-for-cyber-common-issues-and-troubleshooting

## Usage policy on authorized pentest

- **Authorized pentest of your own systems is policy-compliant; the permissive tooling is what's gated.** TAC is "intended for authorized defensive cybersecurity work on systems... you own, operate, or are explicitly authorized to test or analyze." Without TAC, standard-model classifiers will refuse much offensive-security work even though the activity is permitted — so the gate is a **capability/friction gate, not a permission gate**. (as of 2026-07-21, source: https://help.openai.com/en/articles/20001258-openai-daybreak-trusted-access-for-cyber-overview)
- Baseline prohibition: facilitating "dual-use cyber activities carried out with malicious intent, without proper authorization, or in excess of granted authorization." Even inside TAC, hard safeguards still block credential theft, stealth/persistence, malware deployment, and exploitation of third-party systems. (as of 2026-07-21, source: https://openai.com/policies/usage-policies/)

## Deployment surfaces

- **The cyber-permissive tier is granted at the OpenAI org/workspace level** via the channels above — it lives on OpenAI first-party, not Azure. (as of 2026-07-21, source: https://openai.com/index/gpt-5-5-with-trusted-access-for-cyber/)
- **Azure OpenAI / Microsoft Foundry:** standard GPT-5.5 is GA there, but **GPT-5.5-Cyber has no confirmed Azure/Foundry self-serve or Limited-Access SKU** — the cyber tier is an OpenAI-first-party TAC grant. Treat Azure as the surface for *standard* GPT-5.5. (as of 2026-07-21, source: https://azure.microsoft.com/en-us/blog/openais-gpt-5-5-in-microsoft-foundry-frontier-intelligence-on-an-enterprise-ready-platform/)
- Azure "Limited Access" is a separate, real registration — but it governs Azure's *own* higher-risk capabilities (modifying content filters, disabling abuse monitoring, restricted models), not OpenAI's cyber tier. Docs: https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/limited-access (allow ~10 business days). (as of 2026-07-21)

Implication: apply via the **enterprise TAC form**, request the GPT-5.5-Cyber tier explicitly, and don't expect Azure GA access to substitute for the first-party grant.

Verification caveat: all `openai.com`/`chatgpt.com` pages return HTTP 403 to automated fetch (site-wide bot block) — URLs confirmed via search indexing + multiple independent sources, not by rendering the pages directly. The Aardvark→Codex Security rebrand is press-reported, not first-party-confirmed.
