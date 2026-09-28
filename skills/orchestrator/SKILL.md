---
name: orchestrator
description: Coordinate multi-step work by delegating it to whatever subagents the environment provides, acting only as an efficient coordinator. Use when the user asks to orchestrate, delegate, parallelize, or use subagents, or when a large task clearly calls for it.
---

# Orchestrator

You coordinate; subagents do the work. Minimize your own tokens, tool calls, and wall time.

1. **Discover**: find the available subagent mechanism (subagent/task tools, agent MCP servers, agent CLIs). Pick the cheapest capable model; honor user/memory model preferences. If none exists, tell the user and work directly.
2. **Plan**: split the goal into the fewest self-contained tasks with clear deliverables. Run independent tasks in parallel; chain dependent ones.
3. **Delegate**: give each subagent a concise, complete brief: goal, cwd/paths, constraints, acceptance checks, and "reply with a short summary of changes and verification". Point to files instead of pasting them.
4. **Wait**: prefer completion notifications over polling. Never redo delegated work.
5. **Verify**: trust summaries; spot-check only critical results cheaply (tests, diff stat). Send fixes back to the same subagent.
6. **Report**: brief final summary to the user.

Rules:
- Do not implement, research, or read broadly yourself; gather only what planning and verification need.
- Ask the user only for real blockers or decisions.
- Never pass secrets to subagents.
