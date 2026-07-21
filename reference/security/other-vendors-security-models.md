---
topic: Other security-model channels — Google, Microsoft Security Copilot, and self-hosted open-weight
volatility: check-monthly
related: 00-ai-pentest-access.md, authorization-and-compliance.md
---

# Other channels: Google · Microsoft · self-hosted

The non-Anthropic/non-OpenAI options. Most are defensive/SOC-oriented, not turnkey offensive pentest agents — noted where so.

## Google

- **Sec-Gemini v3** — Google's specialized cybersecurity agent (incident investigation, forensics, malware analysis; continuously ingests Google Threat Intelligence). **Limited to a trusted-tester program for government and enterprise — not GA, no self-serve URL.** Sales/trusted-tester gated. (as of 2026-07-21, source: https://siliconangle.com/2026/07/21/google-expands-gemini-cheaper-models-bug-hunter-keeps-leash/)
- **CodeMender** — scan → prove-via-sandbox-exploit → patch as code diff (C/C++, Go, Java, Python, Ruby, Rust, TypeScript). **Entered preview 2026-07-21.** Usable with GA Gemini models via the Gemini Enterprise Agent Platform or as a component of AI Threat Defense; the higher-capability **Gemini 3.5 Flash Cyber** variant is restricted to a small set of governments/trusted partners. Nearest-term enterprise-accessible Google offensive-security agent. (as of 2026-07-21, source: https://cloud.google.com/blog/products/identity-security/find-and-fix-software-vulnerabilities-with-codemender)
- **"Big Sleep"** (DeepMind + Project Zero) is an internal autonomous vuln-discovery program, **not customer-purchasable** — capability signal only. (as of 2026-07-21, source: https://blog.google/innovation-and-ai/technology/safety-security/cybersecurity-updates-summer-2025/)
- **The GA, buyable Google path is Gemini in Google Security Operations (SecOps) + Google Threat Intelligence** — NL search, case summarization, detection-rule/playbook creation. Full Mandiant+VirusTotal intel is Enterprise-Plus-gated. Defensive/SOC, not offensive. https://cloud.google.com/security/products/security-operations (as of 2026-07-21, source: https://docs.cloud.google.com/chronicle/docs/secops/secops-packages)

## Microsoft — the incumbent path (M365 tenant already in place)

- **Microsoft Security Copilot** is embedded across Defender/Sentinel/Intune/Entra for incident summarization, investigation, threat hunting, KQL/script analysis — **primarily defensive/SOC, not a turnkey offensive pentest agent.** https://www.microsoft.com/en-us/security/pricing/microsoft-security-copilot (as of 2026-07-21, source: https://www.microsoft.com/en-us/security/pricing/microsoft-security-copilot)
- **Pricing: consumption-based Security Compute Units (SCUs) — $4/hr provisioned, $6/hr overage.** One SCU 24/7 ≈ $2,920/mo (~$35k/yr). (as of 2026-07-21, source: https://learn.microsoft.com/en-us/copilot/security/security-compute-units-capacity)
- **M365 E5 now includes Security Copilot capacity free: 400 SCUs/month per 1,000 paid E5 licenses (capped at 10,000/mo)** — announced Ignite 2025, rollout began 2026-11-18. At ~1,000 knowledge workers this is a material free allocation **if the org holds E5** (confirm E5 vs E3 — the whole free-capacity argument hinges on it). Overage currently throttles rather than bills. (as of 2026-07-21, source: https://learn.microsoft.com/en-us/copilot/security/security-copilot-inclusion)
- **For red-teaming AI systems specifically, use Microsoft PyRIT** (Python Risk Identification Tool) — open-source, self-hostable: https://github.com/microsoft/PyRIT. Automates adversarial probing of GenAI endpoints (OpenAI, Azure, Anthropic, Google, HuggingFace, custom HTTP/WebSocket, Playwright web targets); now integrated into Foundry as the **AI Red Teaming Agent**. Tests *AI models/apps* for safety risks — not a general network/infra pentest tool. (as of 2026-07-21, source: https://github.com/microsoft/PyRIT)
- Training labs: https://github.com/microsoft/AI-Red-Teaming-Playground-Labs (as of 2026-07-21)

## Self-hosted / open-weight — maximal sovereignty, no vendor filtering

- **For the most sensitive testing, run an open-weight model fully in-environment** — eliminates vendor usage-policy filtering and data egress entirely. Tradeoff: open-weight security models are materially below frontier (Sec-Gemini / GPT-5.5-Cyber / Opus-class) on reasoning depth and multi-step exploit chaining. Good for air-gapped, no-egress work; not a frontier substitute. (as of 2026-07-21, source: https://www.securityweek.com/whiterabbitneo-high-powered-potential-of-uncensored-ai-pentesting-for-attackers-and-defenders/)
- **WhiteRabbitNeo** — uncensored open-source model series built for red/blue-team use (writes/tests exploit code + remediation). Real weights on HuggingFace:
  - https://huggingface.co/WhiteRabbitNeo/Llama-3.1-WhiteRabbitNeo-2-8B (current 8B, Llama-3.1)
  - https://huggingface.co/WhiteRabbitNeo/WhiteRabbitNeo-13B-v1
  - 33B GGUF (community): https://huggingface.co/manuelgutierrez/WhiteRabbitNeo-33B-v1.5-GGUF · Ollama: `monotykamary/whiterabbitneo-v1.5a`
  - (as of 2026-07-21, source: https://huggingface.co/WhiteRabbitNeo/Llama-3.1-WhiteRabbitNeo-2-8B)
- **Deep Hat** (https://www.deephat.ai/) appears as a successor/rebrand in the WhiteRabbitNeo lineage — **needs verification before relying on specifics.** (as of 2026-07-21, source: https://www.deephat.ai/)
- General strong open-weight models (Llama-family, etc.) can be self-hosted and driven with PyRIT/agent harnesses; the security-tuned models above mainly remove refusal friction on offensive prompts.

Implication for our profile: Security Copilot is the near-zero-friction *defensive* option already in the tenant; a self-hosted WhiteRabbitNeo instance is the option for testing that must never leave a Canadian-controlled environment. Neither replaces a frontier CVP/TAC grant for hard offensive-security reasoning.
