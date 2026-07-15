---
topic: Agentic coding method — plan → task register → one task per context window (mid-2026 practice)
volatility: check-quarterly
related: copilot-vscode-plan-mode.md, copilot-vscode-daily-workflow.md, copilot-vscode-agents-subagents.md, ../5.6-use-guide/00-guide.md
---

# Agentic coding method — the working loop

The house method for agentic coding in Copilot/VS Code, synthesized 2026-07-15 from Anthropic's Claude Code best practices, VS Code docs, and practitioner writing. Deck: `decks/copilot-vscode-quickstart.html`.

## The method (golden standard)

1. **Plan** — decompose the problem into tasks/subtasks with a planning step before any code. Anthropic's canonical workflow is Explore → Plan → Code → Commit; "letting Claude jump straight to coding can produce code that solves the wrong problem." (as of 2026-07-15, source: https://code.claude.com/docs/en/best-practices)
2. **Persist the register** — write the plan to a markdown task register in the repo (plan.md / tasks.md, checkbox list + notes). Anthropic documents the equivalent directly: write the spec to a file, then "start a fresh session to execute it… clean context focused entirely on implementation, and you have a written spec to reference." Structured note-taking (NOTES.md, to-do lists persisted outside the window) is a named context-engineering pattern. (as of 2026-07-15, sources: same · https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, 2025-09-29)
   - **Copilot caveat:** the VS Code Plan agent saves to session memory (`/memories/session/plan.md`) which is wiped when the conversation ends; writing to a repo plan.md is a user instruction ("write this plan to plan.md as a checklist"), not documented VS Code behavior. (as of 2026-07-15, source: https://code.visualstudio.com/docs/agents/planning)
3. **One task per context window** — implement a single task/subtask per fresh window; update the register (check off, append learnings) between windows. The register is the state that survives; each window starts clean and reads state from the filesystem (Willison's iterating-agents pattern). (as of 2026-07-15, source: https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/)

## Why: context rot

- "As the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases." (2025-09-29, source: Anthropic context-engineering) Independent measurement: 11 of 12 models dropped below 50% of their short-context performance past ~32K tokens. (source: https://arxiv.org/pdf/2606.29718)
- Long sessions also cost more: all accumulated context re-bills on every request, and GPT-5.6 requests past 272K input bill 2× input / 1.5× output ([02-copilot-economics](../5.6-use-guide/02-copilot-economics.md)).

## Building the window at task start

- **Show, don't tell:** point at an exemplar file — "X.php is a good example, follow the pattern" is an explicit Anthropic best-practice row. Reference specific files; don't say "investigate the repo" (unscoped exploration is a named failure mode: hundreds of files read, window filled). (as of 2026-07-15, source: https://code.claude.com/docs/en/best-practices)
- **Instruction files carry conventions** (`/init` generates copilot-instructions.md / AGENTS.md); they load every request, so keep them short — per-line test: "would removing this cause mistakes? If not, cut it." (as of 2026-07-15, sources: same · https://code.visualstudio.com/docs/agents/reference/copilot-vscode-features)
- Target: "the smallest possible set of high-signal tokens that maximize the likelihood of the desired outcome." (2025-09-29, Anthropic context-engineering)

## Context-fill threshold

- **VS Code shows a context gauge**: shaded bar in chat; hover = exact tokens as a fraction (e.g. 15K/128K) + usage breakdown by category. (as of 2026-07-15, source: https://code.visualstudio.com/docs/copilot/chat/copilot-chat-context)
- **House rule: at ~40% fill, plan your exit** — synthesize what the session learned into the register, then start a fresh window. The specific number is house guidance, not vendor doctrine: no vendor publishes a threshold; attributable practitioner numbers include ~60% (Dibia) and Claude Code's ~95% auto-compact; one counter-view argues the right trigger is semantic (safe boundary) not numeric. The direction is undisputed — degradation starts well before the hard limit. (as of 2026-07-15, sources: https://newsletter.victordibia.com/p/context-engineering-101-how-agents · https://blakecrosley.com/blog/agent-context-compaction)
- VS Code auto-compacts "when the context window fills up" (no published %; disable via `github.copilot.chat.summarizeAgentConversationHistory.enabled`). Treat auto-compaction as the emergency fallback, not the plan. (as of 2026-07-15, source: copilot-chat-context doc)

## Mid-2026 refinements worth knowing

- **Subagent restraint:** current guidance flags over-spawning; direct work is often "faster and cheaper." Use subagents for investigation/analysis only; keep edits and approval-gated actions in the parent. (as of 2026-07-15, sources: https://code.claude.com/docs/en/best-practices · https://www.ksred.com/claude-code-agents-and-subagents-what-they-actually-unlock/)
- **Adversarial diff review, calibrated:** review the diff in a fresh context that sees only diff + criteria, but instruct the reviewer to flag only gaps affecting correctness or stated requirements (an eager reviewer always finds something; chasing every finding over-engineers). Never merge on agent review alone. (as of 2026-07-15, source: https://code.claude.com/docs/en/best-practices)

## VS Code command facts (settled 2026-07-15)

- **Edit mode is deprecated**: hidden by default since v1.109 (Jan 2026), deprecated v1.110, removal scheduled ~v1.126 (`chat.editMode.hidden` restores it meanwhile). Use Agent for multi-file edits. (sources: https://code.visualstudio.com/updates/v1_109 · v1_110 · v1_111)
- **`/fork` is real** (since v1.111, Mar 2026): branches the session into an independent copy with full history. **`/resume` is NOT a VS Code slash command** (Claude Code CLI flag conflation). Current real set: /plan /compact /clear /fork /fix /tests /init /new. (as of 2026-07-15, source: https://code.visualstudio.com/docs/agents/reference/copilot-vscode-features)

## Unverified

- Edit-mode removal "in v1.126" is scheduled per forward-looking notes, not confirmed shipped.
- VS Code auto-compaction trigger percentage — unpublished.
- 40/50/70% fill thresholds circulating in posts have no locatable primary source; only ~60% (Dibia) and 95% (Claude Code auto-compact) are attributable.
