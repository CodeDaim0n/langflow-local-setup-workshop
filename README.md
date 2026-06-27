# Langflow Local Setup — Workshop

Run [Langflow](https://www.langflow.org/) on Windows or macOS, connect a model provider (OpenAI or a local [Ollama](https://ollama.com/) model), expose your machine to [Camunda 8 SaaS](https://console.camunda.io/) through a [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/), and call your flow with a Langflow API key.

This is the optional **local AI track** for the [Business Loan Onboarding & Verification workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop). Use it when you want to build AI flows on your own machine instead of calling a hosted model directly from Camunda.

> Use **synthetic test data only**. Keep all API keys out of Git, screenshots, and shared documents.

---

## Table of contents

- [What you will set up](#what-you-will-set-up)
- [Repository structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Getting started](#getting-started)
- [What the guide covers](#what-the-guide-covers)
- [Environment variables](#environment-variables)
- [Security and safe data](#security-and-safe-data)
- [Reference documentation](#reference-documentation)
- [License](#license)

---

## What you will set up

```text
   Camunda 8 SaaS
        │  HTTPS request + Langflow API key (x-api-key)
        ▼
 Cloudflare Tunnel (public HTTPS address)
        │
        ▼
 Langflow on your machine  →  http://localhost:7860
        │
        ▼
 Model provider: OpenAI API  or  local Ollama model
```

---

## Repository structure

```text
langflow-local-setup-workshop/
├── README.md
├── LICENSE
├── .env.example                # Reference environment variables (copy to .env)
├── docs/
│   └── local-setup-guide.md    # Full setup: Desktop, venv, Ollama, tunnel, Camunda REST
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
| Camunda 8 account | Only if you connect the flow to Camunda ([console.camunda.io](https://console.camunda.io/)) |

The simplest route is [Langflow Desktop](https://www.langflow.org/desktop), which manages Python and dependencies for you.

---

## Getting started

```bash
git clone https://github.com/CodeDaim0n/langflow-local-setup-workshop.git
cd langflow-local-setup-workshop
```

1. Follow the [local setup guide](./docs/local-setup-guide.md).
2. Confirm Langflow opens at `http://localhost:7860`.
3. Build and test a flow in the Playground.
4. Create a **Langflow API key** for external calls.
5. To connect Camunda, store the key as the `LANGFLOW_API_KEY` Connector Secret and expose Langflow with a Cloudflare Tunnel.
6. Optionally export your flow JSON to [`flows/`](./flows/) — without any keys.

---

## What the guide covers

| Topic | Section in the guide |
|---|---|
| Langflow Desktop (simplest route) | Route A |
| Python `uv` virtual environment | Route B |
| OpenAI API key | Model provider setup |
| Ollama + Qwen (optional local models) | Zero-cost local models |
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

## Security and safe data

- Never commit `.env`, OpenAI keys, or Langflow API keys.
- Do not embed API keys inside exported flow JSON.
- Use HTTPS through Cloudflare for any Camunda → Langflow call.
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
