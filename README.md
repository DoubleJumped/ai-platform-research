# AI Platform Research

A research corpus on enterprise AI platforms and AI coding tools: what models are available, deployable where, in what configuration — plus the decks derived from that research.

Split out of the [ai-work-os](https://github.com/DoubleJumped/ai-work-os) template repo (2026-07-14) so the reusable work-OS template and this living research corpus can evolve separately.

## Framing

- **No hard vendor decision has been made.** Files that recommend a direction are **preview paths** — each lane worked out far enough to move fast if picked. Current deployed state where relevant: GitHub Copilot (coding) + Microsoft Cowork (knowledge work).
- Context profile used throughout: a Canadian Crown corporation, ~1,000+ knowledge workers, ~15 heavy coding-agent users, existing M365 tenant. **This is a public repo: no employer names, colleague names, or internal data — ever.**
- Every volatile fact is stamped `(as of YYYY-MM-DD, source: url)`. Newer stamp wins on contradiction; correct the older file when found.

## Map

| Path | What lives there |
|---|---|
| `reference/` | The corpus — one topic per file, indexed in [reference/INDEX.md](reference/INDEX.md) |
| `reference/5.6-use-guide/` | GPT-5.6 model-selection manual (which model, which effort, which task) |
| `reference/platforms/` | Per-platform reference cards: Microsoft stack (Copilot + Cowork + Studio/Foundry), ChatGPT Enterprise/Business, Claude Enterprise/Cowork |
| `reference/coding-tools/` | Per-tool reference cards: GitHub Copilot, Claude Code, Codex |
| `decks/` | Self-contained HTML decks derived from the research |

Start at [reference/INDEX.md](reference/INDEX.md).
