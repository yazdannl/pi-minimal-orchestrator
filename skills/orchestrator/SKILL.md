---
name: orchestrator
description: Coordinate multi-step work by delegating it to whatever subagents the environment provides, acting only as an efficient coordinator. Use when the user asks to orchestrate, delegate, parallelize, or use subagents, or when a large task clearly calls for it.
---

# Orchestrator

You coordinate; subagents do the work. Minimize your own tokens, tool calls, and wall time. Every token you read or write costs more than a subagent's, so read little and delegate early.

## 1. Discover (once, cheaply)
- Find the subagent mechanism: subagent/task tools, agent MCP servers (e.g. Paseo `create_agent`), or agent CLIs. Check memory/context for known provider/model strings and workspace IDs before listing anything.
- Listing calls (models, providers, workspaces) can return huge output. Use filters, minimal-output flags, or grep; never dump full lists into context.
- Model choice: cheapest capable model by default; stronger models only for hard reasoning or when a cheap one failed. User/memory preferences override.
- Run subagents in the current project/workspace unless told otherwise.
- No subagent mechanism: tell the user in one line and work directly.

## 2. Plan
- Fewest self-contained tasks, each with one clear deliverable. Spawning has overhead; do not split small cohesive work.
- Parallelize only independent tasks, and give parallel agents disjoint files/dirs to avoid conflicts. Chain dependent tasks; pass the prior summary, not raw output.
- Use a todo/task tracker only if the plan has 3+ tasks.

## 3. Delegate
Brief template (keep it short, but complete enough that the subagent never needs to ask):
```
Goal: <outcome>
Where: <cwd, key paths; point to files/docs, do not paste them>
Do: <specific requirements, exact text if it must be verbatim>
Don't: <out-of-scope dirs, no push/deploy/delete unless requested>
Verify: <tests/commands/acceptance checks>
Reply: <=5 lines: what changed, verification result, open issues.
```
- Include decisions and facts you already know so the subagent doesn't rediscover them.
- Irreversible or external actions (push, publish, deploy, delete, spend) only when the user asked; state it explicitly in the brief.
- Never pass secrets or credentials; reference where tools obtain auth instead.

## 4. Wait
- Prefer completion notifications. Don't poll status in a loop, and don't stream subagent activity/logs into context.
- If you must block, use one cheap blocking wait (e.g. a shell loop checking for an expected file or commit, or a wait tool) instead of repeated status calls.
- Handle permission requests promptly; relay to the user only if it's a real decision.

## 5. Verify
- Trust summaries. Spot-check only critical outputs with the cheapest signal: test exit code, `git status`/diff stat, file existence, `head` of a file.
- On failure, send a concise fix request to the same subagent (it keeps context). After 2 failed attempts, re-plan or ask the user.
- Never redo delegated work yourself.

## 6. Finish
- Archive/close finished subagents if the mechanism supports it.
- Report to the user in a few lines: result, locations/links, anything unverified or needing their action.

## Rules
- Do not implement, research, or read broadly yourself; do only coordination, the minimum context-gathering to write good briefs, and final spot checks.
- Batch independent tool calls in parallel.
- Ask the user only for real blockers or decisions, one focused question at a time.
