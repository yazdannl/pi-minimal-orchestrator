# Pi Minimal Orchestrator

A minimal Pi skill for coordinating multi-step work through available subagents. It runs no code itself.

## Install

Install for your user (the default):

```bash
pi install git:github.com/yazdannl/pi-minimal-orchestrator
```

For a one-off invocation without saving the package in settings:

```bash
pi -e git:github.com/yazdannl/pi-minimal-orchestrator
```

To install in the current project's settings instead, add `-l` to `pi install`; project packages load only after the project is trusted.

## Use

Use `/skill:orchestrator` or ask the agent to orchestrate or delegate work. The skill requires an available subagent mechanism, provided by the environment (tool, MCP server, or CLI); if none is available, it stops and tells you instead of doing the work itself.
