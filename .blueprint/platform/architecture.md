---
title: Architecture
version: 1.0.0
status: active
owner: project
created: 2026-10-14
last_updated: 2026-10-14
category: platform
tags: [architecture, adk, mcp, a2a, skills]
confidence: verified
verified_against: 'atlas-agents-workshop working tree, 2026-10-14'
---

# Architecture

```
browser ── atlas/app.py (Flask, :5000) ── recommendation_agent
                                              │
                                              ├─ MCP ──> mcp_server/server.py (:8001/mcp)
                                              │            geocode, nearby_places, walking_route
                                              │            (OpenStreetMap: Nominatim, Overpass, routing)
                                              ├─ A2A ──> description_agent/agent.py (:8002)
                                              │            Google Search on Gemini, Wikipedia on Gemma
                                              └─ skill ─ skills/walking-tour/SKILL.md
```

## Components

| Path (under `demo1-build/`) | Role | Port |
|---|---|---|
| `learn/agent_1_simple` … `agent_7_skills` | Seven lesson agents for `adk web`, one new idea each | 8000 |
| `mcp_server/server.py` | Places tools as an MCP server. Optional Google-account lock via `--allow` or `ATLAS_ALLOWED_EMAILS`. Has a Dockerfile for Cloud Run | 8001 |
| `description_agent/agent.py` | Second agent, exposed over A2A with `to_a2a` | 8002 |
| `skills/walking-tour/` | Walking tour skill, one `SKILL.md`, loaded by the recommendation agent | n/a |
| `atlas/app.py`, `atlas/recommendation_agent.py` | Web app and the agent behind it | 5000 |
| `start.py` | Picks the model, launches the three services, Ctrl+C stops all | n/a |
| `check.py` | Sequential setup check, stops at first failure with a fix | n/a |

## Lesson progression

| Agent | Idea | Needs running |
|---|---|---|
| 1 simple | Model plus instructions | nothing |
| 2 tools | Python functions as tools | nothing |
| 3 mcp | Same tools served by the MCP server | places server |
| 4 mcp_auth | Server only admits named Google accounts | places server, locked |
| 5 remote_mcp | Same server on Cloud Run | deployed server |
| 6 a2a | Description agent over A2A | places server, description agent |
| 7 skills | Walking tour from a skill file | places server |

## Facts that bite

- Service URLs are hardcoded to localhost ports in `atlas/recommendation_agent.py`.
- The web app calls the places server **without a login**, so run the server unlocked for the map.
- Model comes from `ATLAS_MODEL`; `start.py` sets it for the app, `.env` sets it for `adk web learn`.
