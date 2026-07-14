---
topic: Canadian Crown-corporation data privacy & residency requirements for AI adoption
volatility: check-quarterly
related: enterprise-ai-lanes.md
---

# Canadian data requirements (Saskatchewan Crown corporation)

## What the LAW actually requires

- Governing statute: **Saskatchewan FOIP** (c. F-22.01) — Crown corporations are "government institutions" under it. Not LAFOIP (that's municipalities/school boards/health authorities); not PIPEDA (public-body activities are provincially governed). (as of 2026-07-09, source: https://oipc.sk.ca/legislation-main/foip/)
- **There is NO statutory Canadian data-residency requirement in Saskatchewan FOIP.** Storing/processing personal information outside Canada is permitted, not prohibited. (Only Nova Scotia retains a statutory localization rule; BC repealed its own in 2021.) Residency is a **policy preference and risk-assessment item, not a legal blocker** — this is the single most decision-relevant legal fact.

## What the REGULATOR expects (effectively binding in practice)

- SK IPC data-residency guidance: out-of-Canada services are allowed but "the preference will always be to not use such companies"; if used, a **written agreement** covering access/use/storage/protection **and a Privacy Impact Assessment** are required. Core stated risk: foreign lawful access (US CLOUD Act) once data leaves Canada. (as of 2026-07-09, source: https://oipc.sk.ca/data-residency-outside-canada-for-trustees/)
- SK IPC AI guidance: **PIA before adopting AI**, keep PI/confidential data out of AI tools where possible, masking/redaction, monitoring/audit, staff training; endorses the 9 FPT commissioner principles for generative AI. (source: https://oipc.sk.ca/ais-double-edged-sword-balancing-innovation-and-privacy-of-information/)
- Federal Treasury Board **Directive on Automated Decision-Making** does not bind provincial Crowns but functions as the de facto Canadian standard — expect PIA/TRA reviewers to borrow its concepts (algorithmic impact assessment, human-in-the-loop, logging). (source: https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=32592)

## Precedents to cite

- **Alberta (peer Crown government, adopted Claude):** Claude Code via **AWS Bedrock** (enterprise terms, no-training, regional data residency), human review on every patch, standing red/blue security-review agents, AI Academy training. The template for a defensible Canadian public-sector AI program. (source: https://www.anthropic.com/news/alberta-government-claude-cybersecurity)
- **Ontario AG 2026 shadow-AI audit (the cautionary tale):** ~12,000 staff on ~400 AI sites, 60% rated unsafe, 94% of usage on unvetted tools, only 3% trained — because only one sanctioned tool existed. **Lesson: the documented Canadian public-sector AI failure is NOT procuring a vetted tool; it's failing to.** Use this framing in every governance conversation. (source: https://www.auditor.on.ca/en/content/specialreports/specialaudits/en2026/AR_2026_AI_EN.html)

## Vendor residency scorecard (as of 2026-07-09; corrected 2026-07-14 per [canada-residency-paths.md](canada-residency-paths.md))

| | Data at rest in Canada | Inference in Canada | Notes |
|---|---|---|---|
| Microsoft 365 Copilot | **Yes, now** (ADR/Multi-Geo, tenant residency) | **No — announced for 2027** (earlier "2026" date slipped) | Claude-routed Copilot interactions EXCLUDED from in-country commitments; keep Anthropic preview-retention models off |
| OpenAI Enterprise/API | **Yes, now** (Canada region, no extra cost; new workspaces) | **No — defaults to US**; in-region+ZDR exists on API path only | Weakest inference story; expect the most PIA justification |
| Claude first-party | **No — "coming 2026"** | No (US/global only today) | Don't plan a near-term go-live on it |
| Claude via Bedrock ca-central-1 | **Yes — guaranteed** (logs, KBs, config) | **No — flagship Claude models are NOT ca-central-1-native**; callable only via cross-region (geo/global) inference profiles, so inference leaves Canada | The Alberta path; AWS is the processor, Anthropic never sees data. Applies to Claude Code only — **Cowork cannot use Bedrock/Vertex/Foundry backends** (feature request closed "not planned", as of 2026-07-14) |

(sources: https://learn.microsoft.com/en-us/microsoft-365/enterprise/m365-dr-service-copilot · https://help.openai.com/en/articles/9903489 · https://platform.claude.com/docs/en/manage-claude/data-residency · https://aws.amazon.com/blogs/machine-learning/accelerate-generative-ai-innovation-in-canada-with-amazon-bedrock-cross-region-inference/)

## Procurement checklist (what the evaluation will require)

1. **PIA** — the gating document whenever personal information is involved.
2. **Threat-risk assessment** — address US CLOUD Act / foreign lawful access explicitly.
3. **SOC 2 Type II** report from the vendor (all three lanes have it; Anthropic also ISO 27001/17/18, CSA STAR).
4. **DPA + contractual no-training clause** + retention terms + sub-processor disclosure.
5. Documented **risk acceptance** for any cross-border transient inference.

## Bottom line

Residency is **manageable, not blocking**, for every lane — the binding requirements are PIA + written safeguards. The strongest-defensible package today: vetted enterprise tool, no-training default, Canadian (or in-tenant) data-at-rest, humans in the loop, staff training — with cross-border transient inference as a documented, accepted risk where it applies. That posture matches both the SK IPC's guidance and what Alberta actually shipped.
