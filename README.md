# Langflow — No-Code AI Agent Builder

Build, run, and iterate on **AI agents with no code** using [Langflow](https://www.langflow.org/) on your own machine. Langflow stands entirely on its own: you do **not** need a workflow engine, a cloud platform, or any integration to design real, working agents here.

**If you want to learn a no-code AI agent builder that is independent of any workflow engine, this is the track for you.** Connect an agent to a workflow engine later — through [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/) and a [Camunda 8](https://console.camunda.io/) REST call — only if and when you want to embed it in a business process.

> Use **synthetic test data only**. Keep all API keys out of Git, screenshots, and shared documents.

---

## About this workshop

This repository is part of the code-along webinar **“Build a No Code AI Banking Agent.”**

| | |
|---|---|
| **Session** | [Build a No Code AI Banking Agent](https://www.datacamp.com/webinars/build-a-no-code-ai-banking-agent) (free to join) |
| **When** | Monday, June 29, 11 AM ET |
| **Presenter** | **[Anjali Jain](https://www.linkedin.com/in/anjali-jain-42942014/)** — Enterprise AI Architect at Metro Bank, CTO at Erdos Research, and Senior Tutor in AI & Machine Learning at the University of Oxford. Author of *AI-Assisted Programming for Web and Machine Learning* and the forthcoming *Enterprise Architecture in an Agentic World*. |

### Session agenda (60 minutes)

The live session is **45 minutes of content followed by 15 minutes of questions**:

| Segment | Focus |
|---|---|
| Session overview | The complete picture: what we build and why it matters in regulated environments |
| **Langflow** | Build a no-code AI agent — the core agent-building skill, standalone |
| Camunda | Orchestrate that agent inside an auditable business process |
| Q&A | 15 minutes |

You can follow the Langflow portion entirely on its own, even if you never touch a workflow engine.

---

## Table of contents

- [About this workshop](#about-this-workshop)
- [Why Langflow](#why-langflow)
- [What you will build](#what-you-will-build)
- [Repository structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Getting started](#getting-started)
- [What the guide covers](#what-the-guide-covers)
- [Environment variables](#environment-variables)
- [Connecting to a workflow engine (optional)](#connecting-to-a-workflow-engine-optional)
- [Security and safe data](#security-and-safe-data)
- [Reference documentation](#reference-documentation)
- [License](#license)

---

## Why Langflow

Langflow is a **no-code / low-code visual builder for AI agents and flows**. With it you can:

- Assemble agents, prompts, tools, and retrieval visually — no application code required.
- Connect any supported model provider: [OpenAI](https://platform.openai.com/) in the cloud, or a fully local [Ollama](https://ollama.com/) model for zero-cost, private experiments.
- Test agents interactively in the Playground and expose them through a clean API.
- Own the whole loop on your own laptop — build, run, and refine without depending on any external platform.

This is a complete skill in its own right. Everything below works whether or not you ever use Camunda.

---

## What you will build

```text
        Model provider
   OpenAI API  ·  or local Ollama
              │
              ▼
   ┌─────────────────────────┐
   │  Langflow  (your laptop) │   ← build & run agents here, on their own
   │  http://localhost:7860   │
   └─────────────────────────┘
              │
              ▼  (optional) expose via Cloudflare Tunnel
   Connect to a workflow engine such as Camunda 8
```

The Langflow box is the hero. The tunnel and workflow-engine step are optional extensions for when you want an agent to participate in a business process.

---

## Repository structure

```text
langflow-local-setup-workshop/
├── README.md
├── LICENSE
├── .env.example                # Reference environment variables (copy to .env)
├── docs/
│   └── local-setup-guide.md    # Full setup: Desktop, venv, Ollama, Playground, API, tunnel
└── flows/                      # Export your Langflow flow JSON here
    └── README.md
```

---

## Prerequisites

| Item | Minimum |
|---|---|
| Operating system | Windows 10+ or macOS 13+ |
| Python | 3.10–3.14 ([python.org/downloads](https://www.python.org/downloads/)) — only for the Python install route |
| `uv` package manager | [Install guide](https://docs.astral.sh/uv/getting-started/installation/) — only for the Python install route |
| Model access | An [OpenAI API key](https://platform.openai.com/api-keys), or [Ollama](https://ollama.com/) for local models |
| Camunda 8 account | **Only** if you later connect the agent to a workflow engine ([console.camunda.io](https://console.camunda.io/)) |

The simplest route is [Langflow Desktop](https://www.langflow.org/desktop), which manages Python and dependencies for you.

---

## Getting started

```bash
git clone https://github.com/CodeDaim0n/langflow-local-setup-workshop.git
cd langflow-local-setup-workshop
```

1. Follow the [local setup guide](./docs/local-setup-guide.md).
2. Confirm Langflow opens at `http://localhost:7860`.
3. Build an agent and test it in the Playground.
4. Iterate — add tools, prompts, and a model of your choice.
5. Optionally export your flow JSON to [`flows/`](./flows/) (without any keys).

That is a complete, standalone agent-building loop. The next section is only for connecting it to a workflow engine.

---

## What the guide covers

| Topic | Section in the guide |
|---|---|
| Langflow Desktop (simplest route) | Route A |
| Python `uv` virtual environment | Route B |
| OpenAI API key | Model provider setup |
| Ollama + Qwen (local models) | Zero-cost local models |
| Build and test an agent | Playground |
| Expose an agent over HTTP | Langflow API key |
| Cloudflare Tunnel | Quick tunnel and named tunnel |
| Camunda REST Connector | Calling `/api/v1/run/FLOW_ID` |
| Troubleshooting | Connector and tunnel issues |

---

## Environment variables

Copy [`.env.example`](./.env.example) to `.env` and fill in your own values. The `.env` file is git-ignored and must never be committed.

```bash
cp .env.example .env
```

---

## Connecting to a workflow engine (optional)

When you want an agent to run as a step in a business process:

1. Create a **Langflow API key** and call your flow with the `x-api-key` header.
2. Expose Langflow with a [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/) to get a public HTTPS address.
3. Store the key as a Camunda Connector Secret (`LANGFLOW_API_KEY`) and call the flow from a [REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/).

The full Camunda business-process example lives in the companion repository: [business-loan-onboarding-workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop).

---

## Security and safe data

- Never commit `.env`, OpenAI keys, or Langflow API keys.
- Do not embed API keys inside exported flow JSON.
- Use HTTPS through Cloudflare for any external call into Langflow.
- Stop `cloudflared` when you finish (`Ctrl + C`).
- A local tunnel is for development only — never expose it as a production service.

---

## Reference documentation

- [Langflow documentation](https://docs.langflow.org/)
- [Langflow Desktop](https://www.langflow.org/desktop)
- [Langflow API keys and authentication](https://docs.langflow.org/api-keys-and-authentication)
- [Ollama](https://ollama.com/) and [model library](https://ollama.com/library)
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)
- [Camunda REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)

---

## License

See [LICENSE](./LICENSE). These materials are provided for educational use as part of the workshop.
