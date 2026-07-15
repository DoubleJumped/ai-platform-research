---
topic: Copilot in VS Code day-to-day — modes, context controls, session hygiene, credit visibility
volatility: check-monthly
related: copilot-vscode-plan-mode.md, copilot-vscode-agents-subagents.md, ../5.6-use-guide/02-copilot-economics.md, github-copilot.md
---

# Daily workflow — GitHub Copilot in VS Code

## Mode lineup (chat agents dropdown)

- **Completions / Next Edit Suggestions** — inline, always on, **the only free surface** (not credit-billed). (as of 2026-07-15, source: https://github.com/orgs/community/discussions/197089)
- **Ask** — answers/explanations, no edits. **Agent** — hand off a task: plans, picks files, runs tools/terminal, iterates. **Plan** — reviewable plan before edits (see [plan-mode card](copilot-vscode-plan-mode.md)). **Custom agents** — renamed "custom chat modes." **Edit mode is DEPRECATED** — hidden by default since v1.109 (Jan 2026), deprecated v1.110, removal scheduled ~v1.126; use Agent for multi-file edits (`chat.editMode.hidden` restores it meanwhile). (as of 2026-07-15, sources: https://code.visualstudio.com/docs/copilot/chat/copilot-chat · https://code.visualstudio.com/updates/v1_109 · v1_110)
- **Agents window** — dedicated surface for running/monitoring multiple agent sessions (local + background + cloud), parallel side-by-side sessions since the June 2026 release. (as of 2026-07-15, sources: copilot-chat doc · https://github.blog/changelog/2026-07-08-github-copilot-in-visual-studio-code-june-2026-releases/)

## Context controls

- **`#`-mentions are the current idiom**: `#file`, `#codebase` (semantic workspace search — has effectively superseded `@workspace`), folders, symbols, `#terminalSelection`; drag-and-drop files; screenshots/vision. Active file + selection are included implicitly. (as of 2026-07-15, source: https://code.visualstudio.com/docs/copilot/chat/copilot-chat)
- **Instruction files (all current):** `.github/copilot-instructions.md` (project-wide, auto-applied) · `.github/instructions/**.instructions.md` (glob-scoped `applyTo`) · **`AGENTS.md` (stable)** · `CLAUDE.md`/`GEMINI.md` recognized. Encode build/test commands + conventions once instead of repeating per prompt. They're injected into request context, so they consume input tokens on every applicable request (reasoned — docs don't state it). (as of 2026-07-15, source: https://code.visualstudio.com/docs/agent-customization/custom-instructions)

## Session hygiene (quality + cost)

- **New chat per topic.** Official guidance — context accumulates from messages, tool output, and files; long threads inflate every subsequent request. (as of 2026-07-15, source: https://code.visualstudio.com/docs/agents/guides/optimize-usage)
- **`/compact`** manually summarizes the thread (optionally `/compact focus on X`); auto-compaction kicks in when the window fills (no published trigger %). **`/fork`** (since v1.111, Mar 2026) branches the session into an independent copy with full history. **Checkpoints** (on by default) snapshot workspace + conversation for restore. `/resume` is NOT a VS Code slash command (Claude Code CLI conflation — corrected 2026-07-15). Known bug: checkpoint restore can break `/compact` metadata. (as of 2026-07-15, sources: https://code.visualstudio.com/docs/agents/reference/copilot-vscode-features · https://github.com/orgs/community/discussions/177818 · https://github.com/orgs/community/discussions/189562)
- **GPT-5.6 long-context surcharge**: past **272K input tokens** the whole request bills ~2× input / 1.5× output — a long session silently crosses the line. Compact or restart first. (as of 2026-07-15, sources: https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing · https://github.com/openai/codex/issues/32486; consistent with the long-context price table in [02-copilot-economics](../5.6-use-guide/02-copilot-economics.md): $5→$10 in = 2×, $30→$45 out = 1.5×)

## Credit visibility (what a seat-holder can see)

- **VS Code status bar** Copilot dashboard: % of monthly allowance + reset date. **Context-window control hover**: token count + session cost in credits, with per-subagent breakdown. (as of 2026-07-15, source: https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage)
- **github.com/settings/billing → AI usage**: included vs overage, per-model chart. (as of 2026-07-15, source: same)
- **Auto model selection = ~10% credit discount** vs picking the same model manually. (as of 2026-07-15, source: https://github.blog/changelog/2026-07-01-copilot-cli-auto-model-selection-routes-based-on-task/)
- Burns credits fastest: long agent sessions (context growth), high thinking effort (reasoning tokens), big tool outputs, premium models. Full economics: [02-copilot-economics](../5.6-use-guide/02-copilot-economics.md).

## Model picker mechanics

- Switch models **mid-conversation** — no new session needed. **Thinking Effort** submenu behind the `>` arrow next to the model name (reasoning models only); July 2026 merged context-size + effort into one model-customization picker. Effort persists **per model per session**; new conversations inherit your last setting for that model. (as of 2026-07-15, sources: https://code.visualstudio.com/docs/agent-customization/language-models · https://github.com/microsoft/vscode/issues/303685)

## New in June–July 2026 releases (intermediate-user highlights)

- Parallel agent sessions in the Agents window · **agent permission levels per session (Default / Bypass Approvals / Autopilot — Autopilot now ON by default; self-approves and runs to completion — check yours)** · session + per-subagent cost visibility · browser tools GA · model discovery via Language Models editor · session sync/search across machines · 1M-token windows on compatible models · MCP config shared with Copilot CLI. (as of 2026-07-15, sources: https://github.blog/changelog/2026-07-08-github-copilot-in-visual-studio-code-june-2026-releases/ · https://visualstudiomagazine.com/articles/2026/06/11/vsm-vs-code-1-124.aspx)

## Official best practices (docs.github.com, condensed)

- A good agent task = problem statement + acceptance criteria + relevant files + test commands + explicit out-of-scope boundaries. Agree on approach before code (Plan). Review the diff on a branch; treat agent output as a PR to review, not merge blindly. Encode conventions in instruction files. (as of 2026-07-15, source: https://docs.github.com/copilot/how-tos/agents/copilot-coding-agent/best-practices-for-using-copilot-to-work-on-tasks)

## Unverified

- Exact deprecation status of `@workspace`/`@github` participants in VS Code (docs conflate with Visual Studio) — recommend `#codebase` regardless.
- Instruction-file token cost is reasoned, not documented.
- VS Code version→feature mapping for the June 2026 batch is approximate; sources also disagree on current version numbering (1.124 vs 1.128 claims) — cite changelog dates, not versions.
