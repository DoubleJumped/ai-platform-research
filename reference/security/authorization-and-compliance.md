---
topic: Authorization, rules of engagement, and Canadian public-sector compliance for AI-assisted pentesting
volatility: check-quarterly
related: 00-ai-pentest-access.md, anthropic-cyber-verification-program.md, openai-trusted-access-cyber.md, ../canadian-data-requirements.md
---

# Authorization & compliance layer

The paperwork that has to exist *before* any AI-assisted pentest runs — and the thing that actually unlocks the vendor programs. For a Canadian Crown corporation this layer is not optional.

## Why "authorized security testing" is the unlocking phrase

- Every major model vendor prohibits compromising systems **without authorization** — the prohibition is scoped to *unauthorized* access. Documented authorization to test systems you own/operate is what moves the same activity from prohibited to permitted (usually behind a verification program: Anthropic CVP, OpenAI TAC). (as of 2026-07-21, source: https://www.anthropic.com/legal/aup)
- **Common pattern across vendors:** pentest of systems you own or are authorized to test = permitted (often behind a verification tier); unauthorized access = prohibited; top-tier abuse (ransomware, C2, backbone attacks) = blocked for everyone. So the org's documented authorization is a **required input artifact** to the applications, not just internal governance. (as of 2026-07-21, source: https://openai.com/policies/usage-policies/)

## The one prerequisite artifact

- **A documented authorization / rules-of-engagement (RoE) / scope-of-testing document**, naming: systems owned or authorized-to-test, in-scope/out-of-scope targets, time windows, prohibited actions, and named accountable owners. It serves three purposes at once: internal legal authorization, the evidence vendor verification programs ask for, and the "explicit written authorization from the resource owner" that Azure's pentest RoE requires for any third-party tester. **Do not run AI-assisted testing without it.**

## Cloud provider pentest policies (verified)

- **AWS — self-service, no prior approval for a defined service list.** Pentest your own resources without notifying AWS for EC2/WAF/ELB, RDS, CloudFront, Aurora, API Gateway, AppSync, Lambda, Lightsail, Elastic Beanstalk, ECS, Fargate, OpenSearch, FSx, Transit Gateway, and Bedrock AgentCore. **Prohibited:** DoS/DDoS (incl. simulated), port/request flooding, S3/subdomain takeover, Route 53 DNS attacks. Anything else or any DoS needs the Simulated Events form (https://console.aws.amazon.com/support/contacts#/simulated-events, ≥2 weeks ahead). Policy: https://aws.amazon.com/security/penetration-testing/ (as of 2026-07-21, source: https://aws.amazon.com/security/penetration-testing/)
- **Azure / Microsoft Cloud — no pre-approval required since 2017-06-15**, governed by the Microsoft Cloud Unified Penetration Testing Rules of Engagement. Permitted on your own resources: OWASP-Top-10/DAST of your apps & APIs, fuzzing, port scanning, vuln assessment on your VMs, testing your own detection/conditional-access. **Prohibited:** DoS/DDoS (all circumstances), phishing/social-engineering of Microsoft staff, post-compromise actions against Microsoft services. Third-party testers require **explicit written authorization from the resource owner, documented before testing.** DDoS resilience testing must use Microsoft-approved simulation partners. RoE: https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement (as of 2026-07-21, source: https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement)

## Canadian public-sector obligations

- **Privacy Impact Assessment (PIA).** Federal institutions must run PIAs under the TBS Directive on Privacy Practices for new programs/activities using personal information, and update them when introducing new/modified IT — which a frontier AI testing tool is. (as of 2026-07-21, source: https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=32592)
- **Algorithmic Impact Assessment (AIA)** — TBS mandatory 65-risk-question tool where an automated decision system is in scope. Likely scoped out for pure security tooling that feeds no decisioning, but **assess, don't assume**. Existing pre-2025-06-24 systems have until 2026-06-26 to comply. (as of 2026-07-21, source: https://www.canada.ca/en/government/system/digital-government/digital-government-innovations/responsible-use-ai/algorithmic-impact-assessment.html)
- **TBS Guide on the use of generative AI** mandates a staged, risk-based method: identify purpose/risk, consult legal/privacy/security/GBA+ stakeholders, run pre-deployment testing, document approvals. Explicit red lines: **do not enter personal/sensitive info into public third-party tools; use GC-managed instances for sensitive processing; secure non-training / opt-out contractual terms.** Directly relevant to keeping a frontier model from exfiltrating sensitive data during testing. (as of 2026-07-21, source: https://www.canada.ca/en/government/system/digital-government/policies-standards/policy-service-digital-announcements/guide-use-generative-artificial-intelligence-ai.html)
- **Data residency / sovereignty.** For sensitive/protected data, deploy AI within GC-controlled environments enforcing Canadian residency, access controls, and model governance; align to **ITSG-33** via the GC Cloud Security Control Profile, and prefer providers holding **CCCS Medium** assessment. This is the constraint that pushes the most sensitive testing toward Canadian-region hosting or the self-hosted open-weight option. (as of 2026-07-21, source: https://learn.microsoft.com/en-us/compliance/regulatory/offering-cccs-medium)
- **Cyber-specific baseline:** CCCS ITSAP.00.041 (Generative AI) for handling/controls expectations. (as of 2026-07-21, source: https://www.cyber.gc.ca/en/guidance/generative-artificial-intelligence-ai-itsap00041)

Implication: the compliance work (RoE + PIA + genAI-guide checklist) is the gating path, and it overlaps almost entirely with what the vendor programs already ask for — do it once, reuse it for both. See residency detail in [../canadian-data-requirements.md](../canadian-data-requirements.md).
