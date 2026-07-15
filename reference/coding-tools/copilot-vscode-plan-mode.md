---
topic: Copilot Plan agent in VS Code — what it is, mechanics, when to use
volatility: check-monthly
related: copilot-vscode-agents-subagents.md, copilot-vscode-daily-workflow.md, ../5.6-use-guide/00-guide.md, github-copilot.md
---

# Plan agent — GitHub Copilot in VS Code

**Surface warning first:** "plan mode" exists on four surfaces with different mechanics — VS Code (Plan agent, this file), Visual Studio full IDE (persistent `.copilot/plans/*.md` files), github.com cloud agent ("research, plan, and code," GA 2026-04-01), and Copilot CLI (`/plan`, Shift+Tab). Blogs routinely attribute Visual Studio's persistent-plan-file behavior to VS Code — it's wrong. (as of 2026-07-15, sources: https://code.visualstudio.com/docs/copilot/agents/planning · https://devblogs.microsoft.com/visualstudio/plan-before-you-build-introducing-the-plan-agent-in-visual-studio/ · https://github.blog/changelog/2026-04-01-research-plan-and-code-with-copilot-cloud-agent/)

## What it is / how to invoke

- Plan is a **built-in agent** in the VS Code chat agents dropdown (alongside Ask, Edit, Agent). Invoke via the dropdown or **`/plan <task>`**. Shipped stable (introduced ~v1.107 Nov 2025; Explore subagent + plan memory added v1.110 Feb 2026); no preview flag, though some sub-settings are experimental. (as of 2026-07-15, sources: https://code.visualstudio.com/docs/copilot/agents/planning · https://code.visualstudio.com/updates/v1_107 · https://github.blog/changelog/2026-03-06-github-copilot-in-visual-studio-code-v1-110-february-release/)

## Mechanics

- Two-phase: **(1) research & clarify** — read-only codebase exploration + clarifying questions; delegates parallel read-only research to a built-in **Explore subagent** on lightweight models; **(2) plan** — high-level summary + implementation + verification steps. No code changes during planning; iterate via follow-up prompts. (as of 2026-07-15, source: https://code.visualstudio.com/docs/copilot/agents/planning)
- **The plan lives in session memory** — `/memories/session/plan.md`, viewable via "Chat: Show Memory Files." It survives compaction within the session but is **wiped when the conversation ends**. Copy it out if you need it later. This is the #1 gotcha vs Visual Studio's persistent plan files. (as of 2026-07-15, source: same)
- Handoff: **"Start implementation"** button hands the plan to Agent mode; **"Continue in"** can hand off to a background or cloud agent instead. (as of 2026-07-15, sources: planning doc · https://code.visualstudio.com/updates/v1_107)

## Models & effort

- **Plan and implementation models are separately pinnable**: `chat.planAgent.defaultModel` (Plan agent) and `github.copilot.chat.implementAgent.model` (implementation handoff). (as of 2026-07-15, source: planning doc)
- **Reasoning effort is NOT settable per agent** — it's model-picker session state only (microsoft/vscode#313546 open, no PR). So "plan on Sol high, implement on Sol medium" needs a manual effort flip; the model half is declarative, the effort half isn't. (as of 2026-07-15, sources: planning doc · https://github.com/microsoft/vscode/issues/313546)
- Recommended pairing per [00-guide](../5.6-use-guide/00-guide.md): plan on **Sol high/xhigh** (or Fable 5 if judgment matters most), implement on **Sol medium** (or Luna xhigh for bounded pieces).

## When to use vs plain Agent mode

- **Use Plan for:** multi-file features, ambiguous/fuzzy requirements, unfamiliar codebases, anything you'd want a reviewable approach for before edits start. (as of 2026-07-15, sources: planning doc · https://devblogs.microsoft.com/visualstudio/introducing-planning-in-visual-studio-public-preview/)
- **Skip Plan for:** surgical fixes and small known-file edits — Edit mode or a direct Agent prompt is cheaper and faster. (Official Ask/Edit/Agent guidance; predates Plan but the boundary holds.) (source: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/)
- Plan doubles as a **stop condition**: "produce the plan, then stop for feedback" is the pattern that cut Theo's 5.6 token burn 4–5× ([04-community-signal](../5.6-use-guide/04-community-signal.md)).

## Cost

- Planning **bills like normal chat** at the chosen model's token rate under AI-credit billing — no special rate, no discount. Read-only research turns are typically cheaper than edit loops, but a long clarify-iterate thread is still billed context growth. (as of 2026-07-15, source: https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

## Settings

- `chat.planAgent.defaultModel` · `github.copilot.chat.implementAgent.model` · `github.copilot.chat.planAgent.additionalTools` (experimental — extra tools during research). (as of 2026-07-15, source: planning doc)

## Unverified

- Whether hand-edits to the session-memory plan file are honored on "Start implementation" (documented for Visual Studio; VS Code docs only say iterate-via-prompts). Verify in-product.
- Practitioner claim that Plan runs a 4-phase internal subagent workflow — not in official docs.
- No formal "GA" changelog line exists for the VS Code Plan agent; treated as stable/shipped.
