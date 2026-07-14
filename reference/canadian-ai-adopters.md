---
topic: Canadian organizations with documented AI coding-agent/assistant deployments
volatility: check-quarterly
related: alberta-velocity-program.md, enterprise-ai-lanes.md
---

# Canadian AI coding adopters — the citable precedents

For business cases: documented deployments only, with confidence flagged. (as of 2026-07-11)

## Tier 1 — cite these (documented + quantified)

**Shopify — the flagship.** CEO memo (Tobi Lütke, 2025-04-07, public): "reflexive AI usage is now a baseline expectation"; teams must show AI *can't* do a job before requesting headcount. Deliberately multi-tool behind an internal LLM proxy/gateway: **Cursor (~3,000 licenses — 1,500 bought, then 1,500 more immediately), Claude Code, GitHub Copilot (~80% adoption since 2021), Codex — all explicitly named.** VP Eng Farhan Thawar: engineering ~20% more productive (measured as solution quality, not LOC); the quotable cost frame: *"If your engineers are spending $1,000/month more on LLMs and are 10% more productive, that's too cheap."* (sources: https://x.com/tobi/status/1909251946235437514 · https://www.bvp.com/atlas/inside-shopifys-ai-first-engineering-playbook · https://www.firstround.com/ai/shopify)

**Wealthsimple — the closest analog to our 15-user lane.** ~600 engineers: **GitHub Copilot (85% of devs), Cursor (~50%), internal LLM Gateway (500 daily users), CTO-led Claude Code rollout to all engineers** (per Pragmatic Engineer, Feb 2026 — their own blog foregrounds Copilot/Cursor/Gateway), Graphite for AI code review after a 2-month eval. Principle worth quoting: "human-friendly codebases are AI-friendly codebases." (sources: https://engineering.wealthsimple.com/wealthsimples-engineering-ai-strategy · https://newsletter.pragmaticengineer.com/p/measuring-ai-dev-tools)

**CIBC — the bank benchmark.** GitHub Copilot + custom CIBC AI Platform since Aug 2024; **1,700+ developers by Jan 2026; 10–14% productivity gain; 86% of devs report improved daily experience** (Deloitte + Microsoft case studies). (sources: https://cibc.mediaroom.com/2024-08-15-CIBC-Launches-Custom-Built-CIBC-AI-Platform-and-Rolls-Out-GitHub-CoPilot · https://www.deloitte.com/ca/en/about/story/impact/cibc.html · https://www.microsoft.com/en-us/industry/microsoft-in-business/customer-story/2026/01/07/modern-banking-reinvented-cibcs-blueprint-for-successful-ai-transformation/)

**TD Bank.** GitHub Copilot for engineers: pilot found **50% of engineers report up to 20 hrs saved per 2-week sprint; 93% of data engineers equal-or-more productive**; 25,000+ staff on GenAI tools by 2026; $1B/yr AI-value target. (source: https://www.fintech.ca/2026/02/09/how-td-is-scaling-ai-across-banking/)

**Scotiabank.** "Scotia Intelligence" (Apr 2026): 71,000+ employees on assistive AI, **5,500 engineers using AI coding assistance** — good for scale; underlying vendor tool unnamed. (source: https://scotiabank.investorroom.com/2026-04-13-Scotiabank-Launches-Scotia-Intelligence)

## Public sector (beyond Alberta — see [alberta-velocity-program.md](alberta-velocity-program.md) for the anchor)

- **Canada Revenue Agency** — "coding assistants for developers" confirmed (Global News, 2026-04-30); tools unnamed, no metrics. (source: https://globalnews.ca/news/11824044/tax-season-2026-cra-ai/)
- **BC government** — Digital Office GitHub Copilot trial (Mar 2025) inside a 30-person GenAI pilot (4.6 hrs/wk saved across the pilot); no published Copilot-specific results yet. (source: https://digital.gov.bc.ca/2025/03/20/bc-data-service-gen-ai-pilot)
- **Hydro-Québec** — the only Crown-utility finding: press-reported claim of up to **50% developer productivity** from generative AI (Les Affaires, Jun 2024); tool and scale undocumented.
- **Ontario Public Service** — 15,000+ weekly M365 Copilot users, ~3 hrs/wk claimed — but that's office productivity, **not** coding; don't cite it as a coding precedent.

## The Saskatchewan white space (explicit negatives, searched 2026-07)

**No documented coding-assistant deployment exists for any Saskatchewan organization** — SaskPower, SaskTel, Vendasta, 7shifts, Nutrien, Cameco, and the remaining Crown corporations all searched, nothing citable found. Cuts both ways: no local precedent to lean on, but "first documented Saskatchewan Crown to do this properly" is a leadership story for the director — with Alberta next door as the de-risking precedent.

## Honesty flags for any deck

- Scotiabank's and RBC's specific coding-tool vendors are undocumented (RBC: internal "Code Gen Practitioner – Copilot" cert badge implies GitHub Copilot; no metrics). Telus's much-publicized Fuel iX is a general enterprise-assistant platform, **not** developer tooling — don't cite it as a coding precedent.
- University of Waterloo explicitly declines to license GitHub Copilot centrally — a citable counter-example to have an answer for.
- Absence of documentation ≠ absence of adoption; internal tooling choices usually go unpublished. The documented record understates real adoption.

## The pattern worth copying

Every Tier-1 org converged on the same architecture: **multiple coding tools behind one governed internal gateway** (Shopify's proxy, Wealthsimple's LLM Gateway, CIBC's AI Platform) rather than a single-vendor mandate — tool choice per task, governance and spend centralized. That is exactly the P2 harness + P1 platform shape, at bank/Shopify scale.
