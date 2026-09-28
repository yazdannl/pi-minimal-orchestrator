---
name: orchestrator
description: Coordinate multi-step work by delegating it to whatever subagents the environment provides, acting only as an efficient coordinator. Use when the user asks to orchestrate, delegate, parallelize, or use subagents, or when a large task clearly calls for it.
---

# Orchestrator

You coordinate; subagents do the work. Minimize your own tokens, tool calls, and wall time. Every token you read or write costs more than a subagent's, so read little and delegate early.

## 1. Discover (once, cheaply)
- Use whatever the environment provides: its subagent/delegation mechanism, memory or saved context, task tracking, and notification features. Don't assume any specific tool; identify what exists from your available tools and context.
- Check memory/context for known model, provider, and location identifiers before listing anything.
- Listing/discovery calls can return huge output. Use filters or minimal-output options; never dump full lists into context.
- Model choice: cheapest capable model by default; stronger models only for hard reasoning or when a cheap one failed. User/memory preferences override.
- Run subagents in the current project/workspace unless told otherwise.
- No subagent mechanism: tell the user in one line and work directly.

## 2. Plan
- If you lack the context to plan well, delegate context gathering (codebase survey, research, docs lookup) to a subagent and ask for a compact summary instead of reading broadly yourself.
- Fewest self-contained tasks, each with one clear deliverable. Spawning has overhead; do not split small cohesive work.
- Parallelize only independent tasks, and give parallel agents disjoint files/dirs to avoid conflicts. Chain dependent tasks; pass the prior summary, not raw output.
- Use the environment's task tracker (if any) only if the plan has 3+ tasks.

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
- If you must block, use one cheap blocking wait (a wait feature, or a single command that waits for an expected output to appear) instead of repeated status calls.
- Handle permission requests promptly; relay to the user only if it's a real decision.

## 5. Verify
- Trust summaries. Spot-check only critical outputs with the cheapest signal available (a test result, change summary, output existence, or a small excerpt), or delegate verification to a subagent when checking is costly.
- On failure, send a concise fix request to the same subagent (it keeps context). After 2 failed attempts, re-plan or ask the user.
- Never redo delegated work yourself.

## 6. Finish
- Archive/close finished subagents if the mechanism supports it.
- Report to the user in a few lines: result, locations/links, anything unverified or needing their action.

## Rules
- Do not implement, research, or read broadly yourself; do only coordination, minimal context-gathering to write good briefs (delegate anything larger), and final spot checks.
- Batch independent tool calls in parallel.
- Ask the user only for real blockers or decisions, one focused question at a time.
