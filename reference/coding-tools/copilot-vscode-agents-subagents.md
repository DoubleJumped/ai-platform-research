---
topic: Copilot custom agents & subagents in VS Code — config, when they pay off, when to skip
volatility: check-monthly
related: copilot-vscode-plan-mode.md, copilot-vscode-daily-workflow.md, github-copilot.md, ../5.6-use-guide/02-copilot-economics.md
---

# Custom agents & subagents — GitHub Copilot in VS Code

## Custom agents (`*.agent.md`)

- Markdown files with YAML frontmatter; body = instructions/persona. Discovery: workspace `.github/agents/` (also reads `.claude/agents/`), user-profile `~/.copilot/agents`, extra dirs via `chat.agentFilesLocations`. (as of 2026-07-15, source: https://code.visualstudio.com/docs/agent-customization/custom-agents)
- Frontmatter: `name`, `description`, `tools` (default = all incl. MCP), `model` (string or prioritized array), `agents` (subagent allow-list — experimental), `user-invocable`, `disable-model-invocation`, `handoffs` (`{label, agent, prompt, send, model}`), `target`, `argument-hint`, `hooks` (preview). `infer` is deprecated. (as of 2026-07-15, source: same)
- **Model pinnable, effort NOT** — reasoning effort stays session-global picker state (microsoft/vscode#313546 open). Copilot **CLI** does have `reasoningEffort:` frontmatter (~v1.0.66+) — a common source of doc-conflation; that key does nothing in VS Code. (as of 2026-07-15, sources: https://github.com/microsoft/vscode/issues/313546 · https://github.com/github/copilot-cli/issues/2904)
- Create: Configure Chat (gear) → Agent Customizations → Agents → New Agent (Workspace/User); or "Chat: New Custom Agent"; or `/create-agent` for AI-assisted generation. (as of 2026-07-15, source: custom-agents doc)
- Built-ins already in the picker: **Agent**, **Plan**, **Ask**. (as of 2026-07-15, source: https://code.visualstudio.com/docs/copilot/agents/overview)

## Subagents in agent mode

- Subagents = **context-isolated** workers the main agent spawns: each gets only its subtask and a clean context, returns only a final summary. **Agent-initiated — there is no @-mention syntax**; you trigger them by phrasing ("run a subagent to research X", "do these in parallel: 1… 2…") or via a custom agent's `agents`/`handoffs` config. (as of 2026-07-15, source: https://code.visualstudio.com/docs/copilot/agents/subagents)
- **Parallel execution supported** (shipped VS Code 1.109, Jan 2026). Nesting off by default (subagents can't spawn subagents); opt-in `chat.subagents.allowInvocationsFromSubagents` (preview) allows depth ≤5. Per-subagent read-only transcripts appear in the Agents window (preview, Jul 2026). (as of 2026-07-15, sources: subagents doc · https://code.visualstudio.com/blogs/2026/02/05/multi-agent-development)
- Subagent model precedence: caller's explicit choice → subagent's `model:` → main conversation model; **a subagent's model cannot exceed the main model's cost tier**. (as of 2026-07-15, source: subagents doc)
- **Cost: subagent tokens bill like any other tokens** under AI-credit billing (each subagent runs its own tool loop). The old "subagents are free premium requests" loophole (microsoft/vscode#292452) died with per-request billing on 2026-06-01; parallel fan-out now has real, sometimes superlinear, cost. Per-subagent credit breakdown is visible in session cost hover. (as of 2026-07-15, sources: https://github.com/microsoft/vscode/issues/292452 · https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/)

## The four agent surfaces (mental model)

| Surface | Runs | Best for |
|---|---|---|
| Agent mode (local) | your machine, interactive | default: you're steering, synchronous |
| Subagents | inside a local session | decompose/parallelize one task; keep noisy research out of main context |
| Copilot CLI ("background") | your machine, outside VS Code | scripted/terminal-first runs |
| Cloud agent | GitHub Actions infra, async | bounded, well-defined task → reviewable PR; "close the IDE, come back to a PR" |

- Cloud handoff: **"Delegate to coding agent"** in VS Code (or Plan's "Continue in"); the moment the GitHub issue is created, control leaves the IDE. Cloud agent consumes Actions minutes + AI credits. (as of 2026-07-15, sources: https://docs.github.com/copilot/concepts/agents/coding-agent/about-coding-agent · https://github.blog/changelog/2026-02-17-delegate-tasks-to-copilot-coding-agent-from-visual-studio/)

## Do intermediate users need subagents? (honest answer: usually no)

- **Default: one agent session + the Plan agent covers most work.** Subagents mainly solve *crowded context*, not capability — the model doesn't get smarter, its working memory gets cleaner. (as of 2026-07-15, sources: subagents doc framing · https://visualstudiomagazine.com/articles/2026/05/06/ai-subagents-coming-to-visual-studio-ides-copilot.aspx)
- **Worth it when:** (a) a task genuinely splits into independent lanes (parallel research/review/audit); (b) one noisy lane (big log dumps, dependency spelunking) would pollute the main context; (c) you want a specialized persona (read-only reviewer, test-writer) with restricted tools.
- **Not worth it when:** the task is sequential anyway, or small enough for one context — you'd add orchestration overhead and multiply token spend for nothing. GitHub's own multi-agent blog gives no overhead caveat; the "saves premium requests" era claim is dead under token billing — treat marketing skeptically.
- Restraint lever: the "only use subagents if the user explicitly requests them" line is an **OpenAI Codex convention**, not Copilot-enforced; Copilot's enforced equivalents are `disable-model-invocation: true` (agent only runs when user-selected) and the `agents: []` allow-list. (as of 2026-07-15, sources: https://developers.openai.com/codex/subagents · custom-agents doc)

## Unverified

- Exact GA label for core subagents (shipped and undogged since 1.109, but no explicit "GA" changelog line).
- Cloud-agent 59-minute task ceiling — single-source, not re-verified.
- CLI concurrency defaults circulating in posts (`max_threads 6` etc.) appear to mix in Codex defaults — don't cite as Copilot.
