---
title: Guardrails
version: 1.0.0
status: active
owner: project
created: 2026-10-14
last_updated: 2026-10-14
category: platform
tags: [rules, secrets, rate-limits]
confidence: inferred
verified_against: null
---

# Guardrails

- **Never commit `.env`.** It is git-ignored and holds the project id. Only `.env.example` is tracked.
- **Never commit `.venv/`, `.adk/` or `__pycache__/`.** All are git-ignored.
- **Keep OpenStreetMap use light.** About one request a second. This is a workshop, not production.
- **Do not point the map app at a locked places server.** It sends no token.
- **Do not hardcode project ids, emails or tokens** in source or docs.
- **Keep lesson agents small.** Each `learn/agent_N` adds exactly one idea over the previous one.
- **Run `python check.py` after touching setup, `.env` or requirements.**
