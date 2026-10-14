---
title: Tech Stack
version: 1.0.0
status: active
owner: project
created: 2026-10-14
last_updated: 2026-10-14
category: platform
tags: [python, adk, gemini, ollama, mcp]
confidence: verified
verified_against: 'demo1-build/requirements.txt and .env.example, 2026-10-14'
---

# Tech Stack

| Concern | Choice |
|---|---|
| Language | Python 3.11 or newer (`check.py` enforces; `mcp` will not install on 3.9) |
| Agent framework | Google ADK (`google-adk[a2a]`), run with `adk web` |
| Tool protocol | MCP over streamable HTTP (`mcp`) |
| Agent to agent | A2A via ADK `to_a2a` and `RemoteA2aAgent` |
| Web app | Flask |
| Local model routing | `litellm` |
| Map data | OpenStreetMap services, free, about one request per second |
| Config | `.env` loaded with `python-dotenv` |

## Models

| `ATLAS_MODEL` | Where it runs | Needs |
|---|---|---|
| `gemini-3.8-flash` | Vertex AI | Google Cloud project, billing, Vertex AI API, ADC login |
| `ollama_chat/atlas-gemma` | This laptop | Ollama, `ollama pull gemma4:e4b-it-qat`, `ollama create atlas-gemma -f Modelfile` (6 GB) |

On Gemini, thinking is switched off and 429s are retried. The description agent uses Google
Search grounding on Gemini (billed on top of tokens) and English Wikipedia on Gemma.
