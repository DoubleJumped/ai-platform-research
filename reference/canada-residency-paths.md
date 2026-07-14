---
topic: Delivery paths for Claude Cowork/Claude Code and ChatGPT+Codex with Canadian data-at-rest and US inference — availability, costs, and how to get there
volatility: check-monthly
related: canadian-data-requirements.md, enterprise-ai-lanes.md, model-pricing.md, m365-copilot.md
---

# Canada-residency delivery paths: Claude lane vs OpenAI lane

<!-- Synthesized 2026-07-14 from three research passes: (1) this repo's reference/ folder,
     (2) live web verification of Anthropic/AWS/GCP, (3) live web verification of OpenAI/Microsoft.
     All prices USD list. Every volatile fact stamped. Recommendations at the bottom.
     This file intentionally exceeds the ~150-line guideline — it is a decision synthesis, not a single-topic card. -->

## TL;DR

The requirement — **data at rest in Canada, inference in the US acceptable** — is deliverable **today** on both lanes, but through different doors:

- **Claude lane:** Claude **Code** meets it now via **AWS Bedrock ca-central-1 + US-geo inference profile** (the Alberta pattern). Claude **Cowork** cannot meet it yet — cloud sessions store data on Anthropic's US servers with no Bedrock/Vertex/Foundry backend option; the compliant interim is desktop-local Cowork sessions (data stays on-device) while waiting for Anthropic's Canada region ("Coming 2026" on Bedrock, Vertex, and Foundry).
- **OpenAI lane:** ChatGPT **Enterprise with the Canada data-residency region** meets it now — storage at rest in Canada, inference in the US by design, at no extra cost, but it requires the Enterprise tier (a new workspace + sales approval; the widely reported ~150-seat/~$108k-yr contract floor is the real gate, and it is an Enterprise-tier floor, **not** a residency-specific minimum). **Codex honors workspace residency.** ChatGPT Business ($20/seat) does **not** get residency.
- **Microsoft Foundry Canada East** is a real asset on both lanes: Global Standard deployments keep data at rest in the Canada geography while inference runs globally, the GPT-5.6 family and gpt-5.3-codex are deployable there today, Codex CLI can be pointed at it, and Claude Code has first-class Foundry support (`CLAUDE_CODE_USE_FOUNDRY=1`) — pending verification that Claude models are deployable in Canada East.

---

## 1. The requirement, restated (from this repo's prior research)

- Saskatchewan FOIP (c. F-22.01) governs; **there is no statutory Canadian data-residency requirement** — residency is a regulator preference and risk-assessment item, not a legal blocker. (as of 2026-07-09, see canadian-data-requirements.md)
- SK IPC expects: written agreement covering access/use/storage/protection, a **PIA** before adoption, and explicit treatment of US lawful access (CLOUD Act) in the TRA. Cross-border **transient inference is acceptable as a documented, accepted risk** — the posture Alberta actually shipped with Claude Code on Bedrock. (as of 2026-07-09, see canadian-data-requirements.md)
- So the target architecture "at rest in Canada, inference in US" is not a compromise — it is the defensible sweet spot the regulator's own guidance anticipates.

---

## 2. Claude lane — research catalogue

### 2.1 Products and plan pricing (first-party)

- Claude Team Standard: **$20/seat/mo annual ($25 monthly), min 5 / max 150 seats — no Claude Code**. (as of 2026-07-14, source: https://claude.com/pricing, https://support.claude.com/en/articles/9266767)
- Claude Team **Premium: $100/seat/mo annual ($125 monthly)** — includes Claude Code, ~6.25× Pro per-session usage, PAYG top-ups available. Discrepancy: the Claude Code enterprise docs page says "$150/seat (Premium)" — verify with sales which is current. (as of 2026-07-14, sources: https://claude.com/pricing, https://code.claude.com/docs/en/third-party-integrations)
- Claude Enterprise: **~$20/seat platform fee (annual) + ALL usage metered at standard API rates** — no bundled tokens since the Nov 2025–Feb 2026 billing change. Min 20 seats self-serve / 50 sales-assisted. Includes Claude Code + Cowork, SCIM, audit logs, Compliance API, custom retention, CMEK. (as of 2026-07-14, source: https://support.claude.com/en/articles/9797531, https://claude.com/pricing)
- Claude Cowork: GA 2026-04-09 on all paid plans (desktop macOS/Windows); cloud/web/mobile version began beta rollout 2026-07-07. No separate charge — usage metered like everything else on Enterprise. (as of 2026-07-14, sources: https://claude.com/blog/cowork-for-enterprise, https://techcrunch.com/2026/07/07/the-coding-agent-wars-are-spilling-into-the-rest-of-the-office-claude-cowork/)
- Cowork enterprise controls (Apr 2026 release): RBAC/SCIM groups, per-team enablement, group spend limits, analytics dashboard + API, OpenTelemetry SIEM streaming, per-tool MCP controls, MDM keys. (as of 2026-07-14, source: https://claude.com/blog/cowork-for-enterprise)

### 2.2 Data residency reality (first-party = US-only)

- Anthropic workspace geo (data at rest + endpoint processing) supports **only `"us"`** — set at workspace creation, immutable. `inference_geo` supports only `"us"` (a **1.1× price multiplier** on all token categories, Opus 4.6+/Sonnet 4.6+) or `"global"`. **There is no Canada option anywhere first-party.** (as of 2026-07-14, source: https://platform.claude.com/docs/en/manage-claude/data-residency)
- Anthropic's regional-compliance page: regional deployment live for US/Europe/APAC via Bedrock, Vertex, and Microsoft Foundry; **"Canada: Coming 2026" on all three**. (as of 2026-07-14, source: https://claude.com/regional-compliance)
- ZDR exists as an org arrangement on the API, but **Claude Fable 5 requires 30-day retention and is unavailable under ZDR**. (as of 2026-07-14, source: https://platform.claude.com/docs/en/manage-claude/data-residency)
- claude.ai Team/Enterprise (and therefore Claude Code seats and Cowork bought through them) store data on **US infrastructure** — fine for "inference in US," fails "data at rest in Canada." (as of 2026-07-14, source: https://privacy.claude.com/en/articles/7996890)

### 2.3 AWS Bedrock — the workable path today

- **Bedrock does not lag Anthropic releases anymore.** Claude Opus 4.8 (launched on Bedrock 2026-05-28, `anthropic.claude-opus-4-8`, 1M context) and Claude Sonnet 5 (day-one, `anthropic.claude-sonnet-5`) are both on Bedrock, alongside Fable 5, Sonnet 4.6, Opus 4.7/4.6. This settles the repo's prior open question — the "no Opus 4.8/Sonnet 5 on Bedrock" concern is **false** as of mid-2026. (as of 2026-07-14, sources: https://aws.amazon.com/blogs/machine-learning/claude-opus-4-8-is-now-available-on-aws/, https://docs.aws.amazon.com/bedrock/latest/userguide/models-region-compatibility.html)
- **ca-central-1 / ca-west-1:** all flagship Claude models callable, but **only via cross-region (geo or global) inference profiles — there is NO in-region Claude inference in Canada.** This also settles the repo's open question #3: flagship Claude is not ca-central-1-native; inference always leaves Canada. Acceptable here, since US inference is in-scope. (as of 2026-07-14, source: https://docs.aws.amazon.com/bedrock/latest/userguide/models-region-compatibility.html)
- **The mechanism:** call `us.anthropic.claude-opus-4-8` (US geo profile) from ca-central-1 → account resources, logs, S3, knowledge bases, invocation logging all live in ca-central-1; inference routes to ca-central-1/us-east-1/us-east-2/us-west-2 on AWS's private backbone, encrypted in transit; CloudTrail records `inferenceRegion` per request; SCPs can pin allowed destination regions. AWS explicitly positions geo profiles as the data-residency compliance option. (as of 2026-07-14, sources: https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html, https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-anthropic-claude-opus-4-8.html)
- **Bedrock pricing:** Opus 4.8 **$5/$25 per MTok** in/out (matches first-party); Sonnet 5 $3/$15 (intro **$2/$10 through 2026-08-31** first-party — whether the intro rate applies on Bedrock unconfirmed); Haiku 4.5 $1/$5; Fable 5 $10/$50. **Geo (regional) endpoints carry a ~10% premium over global** for Claude 4.5+ — so the US-geo profile from Canada ≈ $5.50/$27.50 for Opus 4.8. Price is billed by source region. (as of 2026-07-14, sources: https://aws.amazon.com/bedrock/pricing/, https://platform.claude.com/docs/en/about-claude/pricing)
- Claude Code → Bedrock is first-class: `CLAUDE_CODE_USE_BEDROCK=1`, `AWS_REGION=ca-central-1`, `ANTHROPIC_MODEL='us.anthropic.claude-opus-4-8'`; auth via AWS creds or Bedrock API keys; corporate proxy and LLM-gateway support built in. What's lost vs seats: the Claude-on-web bundle, Anthropic seat management/usage dashboard (costs move to AWS Cost Explorer), and server-side platform features (web search/fetch, code execution, Files API, Batches, automatic top-level prompt caching — manual caching, thinking, tool use, and 1M context all still work). (as of 2026-07-14, sources: https://code.claude.com/docs/en/amazon-bedrock, https://code.claude.com/docs/en/third-party-integrations, https://code.claude.com/docs/en/feature-availability)

### 2.4 Google Vertex — no Canadian story

- Claude models are current on Vertex (Fable 5, Opus 4.8, Sonnet 5, etc.), but multi-region endpoints are **US and EU only — no `ca` option, no Montreal region** for Claude, and Vertex has no Bedrock-style "store-here / infer-there" construct (endpoint region is both storage and inference). Not a candidate. (as of 2026-07-14, sources: https://platform.claude.com/docs/en/build-with-claude/claude-on-vertex-ai, https://claude.com/regional-compliance)

### 2.5 Microsoft Foundry — the wildcard given our Canada East access

- Anthropic Claude is **GA in Microsoft Foundry** (notably not deployable in EU; **Canada East availability unverified — check the Foundry model catalog in our tenant**). (as of 2026-07-14, source: https://www.infoq.com/news/2026/07/claude-foundry-ga-europe/)
- Claude Code supports Foundry as a backend: `CLAUDE_CODE_USE_FOUNDRY=1`, auth via API key/Entra ID. If Claude models deploy in Canada East with a Global deployment type, that would give data-at-rest in the Canada geography + global inference under our existing Azure agreement — same shape as Bedrock, possibly less procurement friction. **Verify in portal before relying on it.** (as of 2026-07-14, source: https://code.claude.com/docs/en/third-party-integrations)

### 2.6 The Cowork gap (decisive for the Claude lane)

- Cowork **local (desktop) sessions**: agent loop and code execution run on-device in a local VM; data stays on the device/connected folders — but conversation history is local-only, so no central admin retention/export. (as of 2026-07-14, source: https://support.claude.com/en/articles/14479288)
- Cowork **remote/cloud sessions** (the new web/mobile version, beta since 2026-07-07): run in per-session sandboxes on **Anthropic-managed infrastructure with files and conversation data stored on Anthropic's servers — no geographic commitment published; presume US**. (as of 2026-07-14, source: https://support.claude.com/en/articles/14479288)
- **Cowork cannot be pointed at Bedrock/Vertex/Foundry** — the feature request was closed "not planned." Unlike Claude Code, there is no backend override. (as of 2026-07-14, source: https://github.com/anthropics/claude-code/issues/40526)
- Net: Cowork with Canadian data-at-rest does not exist today on any channel. Options are (a) desktop-local sessions only (data on managed devices, inference US — arguably meets the requirement with a PIA note about local storage), or (b) wait for Anthropic's Canada region GA ("2026").

---

## 3. OpenAI lane — research catalogue

### 3.1 Naming, pricing, seat minimums

- **"ChatGPT Work" is not a product.** The lineup is Free/Go/Plus/Pro (individual) and **Business / Enterprise / Edu** (workspace); "OpenAI for Business" is umbrella marketing. ChatGPT Team was renamed **ChatGPT Business** in Aug 2025. (as of 2026-07-14, sources: https://chatgpt.com/business/business-plan/, https://help.openai.com/en/articles/8792828)
- ChatGPT Business: **$20/seat/mo annual ($25 monthly), min 2 seats**, self-serve; includes GPTs, Projects, Company Knowledge, ChatGPT Agent, Deep Research, and Codex. Since 2026-04-02 there is also a cheaper **Codex-only seat type**. (as of 2026-07-14, sources: https://help.openai.com/en/articles/8792828, https://nerova.ai/costs-roi/chatgpt-business-pricing-explained-2026)
- ChatGPT Enterprise: **no list price; reported ~$60/seat/mo (range $45–75), ~150-seat annual-prepaid contract floor ≈ $108k/yr minimum — widely reported but unofficial**. OpenAI also offers credits-based "flexible pricing" for Enterprise/Edu/Business. (as of 2026-07-14, sources: https://www.beam.cloud/blog/chatgpt-enterprise-pricing, https://help.openai.com/en/articles/11487671)
- **Negotiability of the 150-seat floor: no firsthand account found of anyone getting below it.** 2026 licensing-negotiation guides consistently call the 150-seat minimum + annual contract "non-negotiable"; the negotiable levers are per-seat price ($45–75), multi-year term discounts (5–15%), and activation-based true-downs on large deals. The only Reddit datapoint that surfaces (secondhand) confirms $60/seat × 150 × 12mo. Counter-signals are thin (one TechTarget piece disputes a hard minimum; flexible pricing exists officially). **Plan on the ~$108K/yr floor; treat sub-150 as a sales-call question, not an assumption.** (as of 2026-07-14, sources: https://atonementlicensing.com/blog/chatgpt-enterprise-pricing-2026/, https://www.gosearch.ai/faqs/chatgpt-enterprise-pricing-explained-cost-tiers-hidden-fees-gosearch-comparison/, https://bestremotetools.com/chatgpt-enterprise-minimum-seats-and-contract-length-require/)

### 3.2 Data residency (ChatGPT product)

- **Canada is a supported data-residency region**, alongside US/EU/UK/Japan/Korea/Singapore/India/Australia/UAE. (as of 2026-07-14, sources: https://openai.com/index/expanding-data-residency-access-to-business-customers-worldwide/, https://help.openai.com/en/articles/9903489)
- Qualifying plans: **Enterprise, Edu, Healthcare, and the API platform only — ChatGPT Business does NOT qualify**. So residency effectively requires the Enterprise contract. (as of 2026-07-14, source: https://openai.com/index/expanding-data-residency-access-to-business-customers-worldwide/)
- **The 150-seat number, corrected:** no source ties any seat minimum to data residency itself. Residency is **included at no additional cost** for eligible Enterprise/Edu customers, subject to sales approval, and requires provisioning a **new workspace** in-region (no migration of existing workspaces). The ~150 seats is the (unofficial) Enterprise **contract** floor — the tier gate, not a residency gate. (as of 2026-07-14, source: https://help.openai.com/en/articles/9903489)
- Coverage: **storage at rest only** — conversations, uploaded files, custom GPTs, image artifacts, backups stored in-region; **inference runs in the US by default**. Exactly our target posture. A separate "inference residency" feature (in-region GPUs) exists for **US and Europe only — not Canada** — and isn't needed here. (as of 2026-07-14, source: https://help.openai.com/en/articles/9903489)

### 3.3 Codex

- Codex today = one product across **Codex cloud** (hosted agent in ChatGPT/web/iOS, Code Review, Slack/Linear), **Codex CLI**, IDE extension, and the Codex app. Included in every paid tier; Enterprise runs it on flexible/credit pricing. Since 2026-04-02 billing is token-based credits (**1 credit ≈ $0.04**) mapping 1:1 to API prices (GPT-5.6 Sol 125 in / 750 out credits per MTok; Terra 62.5/375; Luna 25/150). Heavy agentic use ≈ **$100–200/dev/mo effective** (repo's prior estimate; deck budgets $150 midpoint). (as of 2026-07-14, sources: https://developers.openai.com/codex/pricing, https://help.openai.com/en/articles/20001106-codex-rate-card)
- **Codex honors workspace data residency** — OpenAI docs list "data and inference residency where available" for Codex under Enterprise/Edu, and the backend actively enforces region authorization for non-US workspaces. Caveat: exactly what Codex-cloud artifacts (repos, containers, task logs) are stored in-region for a Canada workspace is not publicly documented — **get it in writing from OpenAI sales** (pairs with the deck's existing "Pre-signature item #1"). (as of 2026-07-14, sources: https://developers.openai.com/codex/enterprise/admin-setup, https://github.com/DusKing1/opencode-openai-residency)
- Codex CLI can also run against **Azure/Foundry deployments** (officially documented `config.toml` provider setup) and, since Jun 2026, **Amazon Bedrock** as model provider. (as of 2026-07-14, sources: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/codex, https://techpp.app/update-tracker/openai-codex-changelog)

### 3.4 OpenAI API residency (for custom builds, P1/P2)

- **`ca.api.openai.com` exists — Canada is a storage-only API region**: data at rest in Canada, inference in the US (regional *processing* is US/EU/UAE only). Configured per-Project at creation; requires sales approval for advanced data controls and a Modified Retention/ZDR amendment. Covered endpoints: chat/completions, responses, batches, audio, images, embeddings, files, vector stores. Excluded: Assistants API, some previews. (as of 2026-07-14, sources: https://developers.openai.com/api/docs/guides/your-data, https://help.openai.com/en/articles/10503543)

### 3.5 Azure OpenAI / Microsoft Foundry Canada East

- Deployment types: **Global** (inference anywhere), **DataZone** (US/EU/APAC zones — **no Canada zone**), **Regional Standard/Provisioned** (inference in-region). **For ALL types, "data stored at rest remains in the designated Azure geography."** So a **Global Standard deployment on a Canada East resource = at-rest in the Canada geography + inference globally (incl. US)** — our target posture with the biggest model catalog. One docs page parenthetically groups geographies as "Americas" — **get Microsoft's written confirmation that Canada East at-rest = Canada geography**, not Americas. (as of 2026-07-14, source: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- **Models deployable Global Standard in canadaeast** (region table updated 2026-07-08): **gpt-5.6-sol/terra/luna, gpt-5.5, gpt-5.4 family, gpt-5.3-chat, gpt-5.3-codex, gpt-5.2 family incl. gpt-5.2-codex, gpt-5.1 family, gpt-5/-mini/-nano/-pro, o1/o3/o3-mini/o4-mini, gpt-4.1 and gpt-4o families, text-embedding-3**. Not available there: image/audio/realtime models, sora-2, computer-use. Regional (all-in-Canada) inference is limited to PTU reserved capacity topping out at gpt-5.2, so US inference remains part of the design. (as of 2026-07-14, source: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability)
- Pricing: DataZone ≈ +10% over Global; Regional Standard delta disputed (0–10%); Global Batch 50% off. Check the Azure pricing calculator for canadaeast specifics. (as of 2026-07-14, source: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- **Limits:** Foundry is API/agent-platform only — **there is no Azure-hosted ChatGPT UI**. Foundry replaces the ChatGPT app for knowledge workers only via custom agents (Foundry Agent Service — agents publishable into Teams/M365 Copilot, GA Jun 2026) or M365 Copilot itself (Canada at-rest now; in-country processing announced for **Canada in 2027**). (as of 2026-07-14, sources: https://devblogs.microsoft.com/foundry/agent-service-build2026/, https://www.microsoft.com/en-us/microsoft-365/blog/2025/11/04/microsoft-offers-in-country-data-processing-to-15-countries-to-strengthen-sovereign-controls-for-microsoft-365-copilot/)

---

## 4. Recommendations

### Path A — Claude Cowork + Claude Code with Canadian data-at-rest, US inference

**What's deliverable now vs later:** Claude Code — fully deliverable today. Cowork — partially (desktop-local only); full compliance arrives with Anthropic's Canada region ("Coming 2026" on Bedrock/Vertex/Foundry).

How to get there:

1. **Claude Code for the ~15 heavy coders → Bedrock ca-central-1, now.** `CLAUDE_CODE_USE_BEDROCK=1`, `AWS_REGION=ca-central-1`, model pinned to `us.anthropic.claude-opus-4-8` (or `us.anthropic.claude-sonnet-5` for cost). Data at rest (logs, config, KBs) in Canada; inference in US on AWS's backbone; CloudTrail logs the inference region per request; SCP-pin the destination regions. This is the documented Alberta pattern — cite it in the PIA.
2. **Check Microsoft Foundry Canada East for Claude models** (we have tenant access): if deployable there Global Standard, `CLAUDE_CODE_USE_FOUNDRY=1` gives the same posture under the existing Microsoft agreement — potentially the lowest-friction procurement of all. Unverified today; five minutes in the model catalog answers it.
3. **Cowork, interim:** desktop app with **local sessions only** on managed devices — data on-device, inference US. Disable/hold the cloud-session beta (org-wide toggle on Team/Enterprise) until Anthropic publishes a storage-region commitment. Accept the admin gap (no central conversation retention for local sessions) in the PIA, or defer Cowork entirely.
4. **Cowork, target state:** trigger on **"Anthropic Canada region GA"** (claude.com/regional-compliance). When it lands, revisit Claude Team Premium ($100/seat incl. Claude Code) or Enterprise (~$20/seat + metered usage, min 20–50 seats) for the knowledge-worker rollout.
5. **PIA/TRA package:** SOC 2 Type II + ISO 27001 (Anthropic), AWS DPA + no-training terms (AWS is the processor on Bedrock — Anthropic never sees the data), CLOUD Act risk-acceptance paragraph for the US-geo inference profile.

**Cost shape (USD list):**

| Component | Cost |
|---|---|
| Claude Code via Bedrock, Opus 4.8, US-geo profile | $5.50 in / $27.50 out per MTok (10% geo premium); Sonnet 5 $3.30/$16.50 (intro lower to 2026-08-31) |
| Rough per-heavy-dev run rate (agentic use, cache-managed) | ~$100–300/mo token spend — validate in pilot; Alberta reported ~$108K/yr all-in for its program |
| 15 devs, later, on Team Premium seats instead | $100/seat/mo → **~$18K/yr** |
| Cowork (interim, desktop-local on existing paid plans) | no incremental license; Enterprise usage metered at API rates |

**Blockers to name honestly:** no Cowork cloud with Canadian residency on any channel today; Anthropic first-party workspace geo is US-only; ZDR excludes Fable 5.

### Path B — ChatGPT (Business/Enterprise) + Codex with Canadian data-at-rest, US inference

**What's deliverable now vs later:** fully deliverable today — but only at the Enterprise tier. Residency is the tier-gate problem, not a technical one.

How to get there:

1. **Start the OpenAI sales conversation aimed at Enterprise-with-Canada-residency.** Ask in writing: (a) confirmation of at-rest-in-Canada / inference-in-US scope for the ChatGPT product **and Codex cloud artifacts specifically**; (b) the real seat/contract minimum — the 150-seat floor is unofficial and OpenAI's own "flexible pricing" (credits-based) framing says it's negotiable; (c) CAD pricing. This letter is the PIA's central paragraph (deck already flags this as Pre-signature item #1).
2. **Provision a NEW workspace in the Canada region** — residency cannot be retrofitted onto an existing workspace. Do not start a US Business workspace expecting to migrate it; a pilot workspace's data will not move.
3. **Codex comes with it:** included in Enterprise seats on flexible/credit pricing; residency-enforced by the backend for non-US workspaces. Budget ~$150/dev/mo effective for heavy use (≈$27K/yr for 15 devs).
4. **Bridge/fallback if Enterprise minimums don't fit yet:** (a) ChatGPT Business at $20/seat (min 2) for pilot **without** residency — acceptable only with a PIA that treats US storage as an accepted interim risk; or (b) the **API path with `ca.api.openai.com`** (storage in Canada, US inference, ZDR amendment available) for custom builds; or (c) **Azure Foundry Canada East Global Standard** deployments (gpt-5.6 family, gpt-5.3-codex) + Codex CLI pointed at Azure — data at rest in the Canada geography under the existing Microsoft agreement, no OpenAI contract needed at all for the coding lane.
5. **PIA/TRA package:** SOC 2 Type II, no-training default for business plans, 30-day configurable retention, compliance API for audit; CLOUD Act paragraph for US inference; Microsoft-side written confirmation that Canada East at-rest = Canada geography if the Foundry route is used.

**Cost shape (USD list):**

| Component | Cost |
|---|---|
| ChatGPT Enterprise (residency-eligible) | ~$60/seat/mo reported (range $45–75), ~150-seat annual floor ≈ **$108K/yr minimum** — unofficial, negotiate; residency itself free |
| ChatGPT Business (no residency) | $20/seat/mo annual, min 2 — pilot bridge only |
| Codex heavy use | ~$100–200/dev/mo effective (credits, 1 cr ≈ $0.04); 15 devs ≈ $27K/yr at $150 midpoint |
| OpenAI API via ca.api.openai.com | standard API rates (GPT-5.6 Sol $5/$30, Terra $2.50/$15, Luna $1/$6 per MTok); no residency surcharge |
| Azure Foundry Canada East, Global Standard | ≈ API-parity token rates; DataZone-style premiums avoided by using Global; Batch 50% off |

### Side-by-side

| | Claude lane | OpenAI lane |
|---|---|---|
| Coding agent w/ CA at-rest + US inference, today | ✅ Claude Code via Bedrock ca-central-1 (geo profile) | ✅ Codex under Enterprise Canada workspace; or Codex CLI on Azure canadaeast |
| Knowledge-worker product w/ CA at-rest, today | ❌ Cowork cloud = US storage, no override (desktop-local workaround only) | ✅ ChatGPT Enterprise Canada region (storage at rest; inference US) |
| Entry cost for residency-compliant posture | Pay-per-token from seat one (Bedrock), no seat minimum | Enterprise contract, reported ~150 seats / ~$108K/yr (unofficial, negotiable) |
| Latest models in the compliant channel | Opus 4.8, Sonnet 5, Fable 5 — day-one on Bedrock | GPT-5.6 Sol/Terra/Luna, gpt-5.3-codex — in ChatGPT and in canadaeast Global |
| Our Foundry Canada East access helps | Maybe — Claude GA in Foundry, canadaeast availability unverified | Yes — full GPT-5.x catalog Global Standard today |
| What unlocks the gap | Anthropic Canada region GA ("2026", Bedrock/Vertex/Foundry) | Negotiating the Enterprise floor down (flexible pricing) |

### Decision triggers (watch monthly)

- **Anthropic Canada region GA** on Bedrock/Vertex/Foundry → Cowork + Team/Enterprise become fully compliant; retire the Bedrock-only constraint. (claude.com/regional-compliance)
- **Cowork cloud storage-region commitment** or a Foundry/Bedrock backend for Cowork (currently "not planned") → re-open Path A step 3.
- **OpenAI Enterprise flexible pricing confirmed below ~150 seats** → Path B entry cost drops materially. (As of 2026-07-14 no firsthand account of a sub-150 deal exists — see §3.1; don't plan on it.)
- **Claude in Foundry canadaeast confirmed** → cheapest-friction coding lane on either path.
- Sonnet 5 intro pricing ends 2026-08-31; M365 Copilot Business promo ends 2026-09-30.

## Verify before relying on

1. Claude model availability in Foundry **Canada East** (portal check — 5 minutes).
2. Claude Team Premium $100 vs $150 docs discrepancy (sales).
3. ChatGPT Enterprise real minimum + CAD pricing + Codex-cloud in-region artifact scope (sales letter).
4. Azure written confirmation: Canada East at-rest = Canada geography (not "Americas").
5. Whether Sonnet 5 intro pricing applies on Bedrock.
6. All figures are USD list — CAD quotes needed for the business case.
