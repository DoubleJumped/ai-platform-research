---
topic: Model API pricing (per million tokens, USD)
volatility: check-monthly
related: model-families.md, enterprise-ai-lanes.md
---

# Model API pricing

All prices USD per MTok. Verified against official vendor pages on the stamp date unless flagged.

## OpenAI — GPT-5.6 family (released 2026-07-09) and current lineup

| Model | Input | Cached input | Output | Batch (in/out) | Context | Notes |
|---|---|---|---|---|---|---|
| GPT-5.6 Sol | $5.00 | $0.50 | $30.00 | $2.50 / $15.00 | 1.05M / 128K out | flagship tier; alias `gpt-5.6` routes here |
| GPT-5.6 Terra | $2.50 | $0.25 | $15.00 | $1.25 / $7.50 | 1.05M / 128K out | balanced default |
| GPT-5.6 Luna | $1.00 | $0.10 | $6.00 | $0.50 / $3.00 | 1.05M / 128K out | light/cheapest 5.6 tier |
| GPT-5.5 | $5.00 | $0.50 | $30.00 | 50% off | (unverified) | prior flagship |
| GPT-5.5 Pro / 5.4 Pro | $30.00 | — | $180.00 | — | (unverified) | no cached rate published |
| GPT-5.4 | $2.50 | $0.25 | $15.00 | 50% off | (unverified) | |
| GPT-5.4 mini | $0.75 | $0.075 | $4.50 | 50% off | (unverified) | |
| GPT-5.4 nano | $0.20 | $0.02 | $1.25 | 50% off | (unverified) | cheapest OpenAI model |
| gpt-5.3-codex | $1.75 | — | $14.00 | 50% off | (unverified) | coding-specialized |

(as of 2026-07-14, source: https://developers.openai.com/api/docs/pricing and https://developers.openai.com/api/docs/models)
Cache mechanics: pre-5.6 models keep automatic prefix caching with no write charge; **GPT-5.6 family uses explicit cache breakpoints — writes bill at 1.25× input, reads keep the 90% discount, 30-min minimum cache life** (as of 2026-07-14, source: https://openai.com/index/gpt-5-6/). Long-context surcharge: 2× input/output past 272K tokens (Luna 200K). Batch = flat 50% off (batch cells above derived from that rule). Rows marked (unverified) lacked an official context-window listing at check time.

## Anthropic — Claude models

| Model | Input | Output | Cache write 5m / 1h | Cache read | Batch (in/out) | Context | Notes |
|---|---|---|---|---|---|---|---|
| Claude Fable 5 | $10.00 | $50.00 | $12.50 / $20.00 | $1.00 | $5.00 / $25.00 | 1M | most capable GA model |
| Claude Mythos 5 | $10.00 | $50.00 | $12.50 / $20.00 | $1.00 | $5.00 / $25.00 | 1M | same model; approved organizations only |
| Claude Opus 4.8 | $5.00 | $25.00 | $6.25 / $10.00 | $0.50 | $2.50 / $12.50 | 1M | 1M context at standard price |
| Claude Sonnet 5 | $3.00 *(intro $2.00)* | $15.00 *(intro $10.00)* | $3.75 / $6.00 | $0.30 | $1.50 / $7.50 | 1M | intro pricing through 2026-08-31 |
| Claude Sonnet 4.6 | $3.00 | $15.00 | $3.75 / $6.00 | $0.30 | $1.50 / $7.50 | 1M | |
| Claude Haiku 4.5 | $1.00 | $5.00 | $1.25 / $2.00 | $0.10 | $0.50 / $2.50 | 200K | fastest/cheapest |

(as of 2026-07-09, source: Anthropic model catalog via claude-api skill, cached 2026-06-24; base Opus/Sonnet/Haiku rates corroborated by https://platform.claude.com/docs/en/about-claude/pricing)
Cache mechanics: explicit `cache_control` breakpoints — write 1.25× input (5-min TTL) or 2× (1-hr TTL), read 0.1× input; any prefix byte change invalidates downstream. Batch = 50% off, async (<1h typical). Cache/batch cells are derived from those documented multipliers, not individually quoted.

## xAI — Grok

| Model | Input | Cached input | Output | Context | Notes |
|---|---|---|---|---|---|
| grok-4.5 | $2.00 | $0.50 | $6.00 | 500K | current flagship; 150 req/s, 50M TPM |
| grok-4.3 | $1.25 | (not published) | $2.50 | 1M | larger context, lower price |
| grok-4.20 (reasoning / non-reasoning / multi-agent) | $1.25 | (not published) | $2.50 | 1M | three variants, same price |
| grok-build-0.1 | $1.00 | (not published) | $2.00 | 256K | coding/agent build model |

(as of 2026-07-09, source: https://docs.x.ai/developers/models and https://docs.x.ai/developers/models/grok-4.5)
Cache mechanics: cached input billed at the discounted rate (25% of input on grok-4.5); no separate write charge or batch tier published.

## Cross-vendor implications

- Cheap-tier drafting (the E3-style question): GPT-5.6 Luna ($1/$6) and Claude Haiku 4.5 ($1/$5) are near-parity; Grok 4.5 sits between tiers ($2/$6).
- Flagship coding/agents: Opus 4.8 ($5/$25) vs GPT-5.6 Sol ($5/$30) — same input, Sol 20% pricier out.
- Anthropic's cache-read discount (90%) is the deepest, but requires explicit cache management; OpenAI's is automatic.
