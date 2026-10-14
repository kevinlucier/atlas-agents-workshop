---
title: Development and Cold Start
version: 1.0.0
status: active
owner: project
created: 2026-10-14
last_updated: 2026-10-14
category: platform
tags: [setup, cold-start, gcloud, venv, troubleshooting]
confidence: verified
verified_against: 'atlas-agents-workshop working tree, 2026-10-14'
---

# Development and Cold Start

From a machine with nothing configured to Atlas on screen. Everything runs from `demo1-build/`.

## 0. Prerequisites

| Need | Check | Install |
|---|---|---|
| Python 3.11+ | `python3 --version` | macOS: `brew install python@3.13`. The system 3.9 is too old |
| Git | `git --version` | Xcode tools or `brew install git` |
| Google Cloud CLI | `gcloud --version` | `brew install --cask google-cloud-sdk` |
| Ollama (only for Gemma) | `ollama --version` | https://ollama.com |

## 1. Python environment

```bash
cd demo1-build
python3.13 -m venv .venv          # use any 3.11+ interpreter
source .venv/bin/activate         # Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
cp .env.example .env
```

## 2. Google Cloud (skip if using Gemma, see step 3b)

A project that exists in **your** Google account, with billing linked, is required. The id in
`.env.example` is a placeholder.

```bash
gcloud auth login                                    # the CLI itself
gcloud auth application-default login               # what the code uses (ADC); separate login
gcloud projects create <unique-id> --name="Atlas Workshop"      # or use the console
gcloud billing accounts list
gcloud billing projects link <unique-id> --billing-account=<ACCOUNT_ID>
gcloud services enable aiplatform.googleapis.com --project <unique-id>
gcloud auth application-default set-quota-project <unique-id>
```

Then set `GOOGLE_CLOUD_PROJECT=<unique-id>` in `.env`. Wait a minute or two after enabling the API.

## 3a. Verify

```bash
python check.py
```

Every line must be `[ ok ]`: Python, packages, `.env` project, gcloud, ADC sign-in, a real
Gemini call, the map service.

## 3b. Alternative: no Google Cloud

```bash
ollama pull gemma4:e4b-it-qat
ollama create atlas-gemma -f Modelfile
```

Set `ATLAS_MODEL=ollama_chat/atlas-gemma` in `.env`, or answer `gemma` to `start.py`.

## 4. Run

| Goal | Command | URL |
|---|---|---|
| Whole app | `python start.py gemini` (or `gemma`) | http://localhost:5000 |
| Lessons 1 and 2 | `adk web learn` | http://localhost:8000 |
| Lesson 3, 7 | also `python mcp_server/server.py` | |
| Lesson 4 | `python mcp_server/server.py --allow you@gmail.com`, test with `python mcp_server/knock.py [--login]` | |
| Lesson 6 | also `python description_agent/agent.py` | |

`adk web` serves all seven agents. Pick one from the dropdown. Keep the terminal open;
Ctrl+C stops the server.

## 5. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `No matching distribution found for mcp` | Python older than 3.10 | Recreate the venv with 3.11+ |
| `403 PERMISSION_DENIED … aiplatform.googleapis.com … disabled` | Vertex AI API off for the project | Enable it, wait a few minutes |
| `gcloud services enable` fails, "not authenticated" | Only ADC login done | `gcloud auth login` |
| Billing error on enable | No billing account linked | Link one in the console |
| `[Errno 48] address already in use` on 8000 | `adk web` already running | `pkill -f "adk web"` or just use the running one |
| Page at :8000 will not load | `adk web` not started | Start it, keep terminal open |
| Map app cannot call tools | Places server locked | Run it without `--allow` |
| Agent errors only on a lesson | Its service is not running | See the run table above |

## 6. Stop

Ctrl+C in each terminal. `start.py` stops all three services together.
