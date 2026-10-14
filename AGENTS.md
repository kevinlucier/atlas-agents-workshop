# AGENTS.md

Entry point for AI agents and developers working in **atlas-agents-workshop**.

Deliberately thin. Detail lives in [`.blueprint/`](.blueprint/) and is not repeated here.

## What this is

A workshop that builds **Atlas**: a map agent that finds places near you, describes them
from checked sources, and draws the walking route. Each step adds one idea: an agent,
tools, MCP, MCP auth, A2A, skills. All code is under `demo1-build/`.

## Read this first

| To understand… | Read |
|---|---|
| **Cold start: get it running from nothing** | [`.blueprint/platform/development.md`](.blueprint/platform/development.md) |
| How the pieces fit and which port is which | [`.blueprint/platform/architecture.md`](.blueprint/platform/architecture.md) |
| Technologies and model options | [`.blueprint/platform/tech-stack.md`](.blueprint/platform/tech-stack.md) |
| Rules to follow | [`.blueprint/platform/guardrails.md`](.blueprint/platform/guardrails.md) |
| Why the project exists | [`.blueprint/product/product.md`](.blueprint/product/product.md) |
| Machine-readable summary | [`.blueprint/agent.yaml`](.blueprint/agent.yaml) |

## Key commands (from `demo1-build/`, venv active)

```
python check.py              # verify setup, stops at first problem
python start.py gemini       # whole app on http://localhost:5000  (or: gemma)
adk web learn                # the seven lesson agents on http://localhost:8000
```
