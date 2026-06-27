# Langflow Local Setup — Workshop

Run **Langflow** on Windows or macOS, connect model providers (OpenAI or local **Ollama/Qwen**), expose your laptop to **Camunda SaaS** via **Cloudflare Tunnel**, and call flows with a **Langflow API key**.

> Separate from the main Camunda workshop repo. Clone this when your track includes local AI flows.

## Related repository

**[business-loan-onboarding-workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop)** — Camunda BPMN, integrations (Companies House, Salesforce, email), and business loan flow guide.

## Repository layout

```text
langflow-local-setup-workshop/
├── README.md
├── docs/
│   └── local-setup-guide.md    ← full setup (Desktop, venv, Ollama, tunnel, Camunda REST)
└── flows/                      ← export Langflow flow JSON here
    └── README.md
```

## Quick start

```bash
git clone https://github.com/CodeDaim0n/langflow-local-setup-workshop.git
cd langflow-local-setup-workshop
```

1. Follow [local setup guide](./docs/local-setup-guide.md).
2. Confirm Langflow at `http://localhost:7860`.
3. Create a **Langflow API key** for Camunda (`x-api-key` header).
4. Store it in **Camunda Connector Secrets** as `LANGFLOW_API_KEY`.
5. Export flows to [`flows/`](./flows/) (optional; no keys in JSON).

## What this guide covers

| Topic | Section in guide |
|---|---|
| Langflow Desktop (beginners) | Route A |
| Python `uv` virtualenv | Route B |
| OpenAI API key | Model provider setup |
| Ollama + Qwen (optional) | Zero-cost local models |
| Cloudflare Tunnel | Quick tunnel and named tunnel |
| Camunda REST Connector | Call `/api/v1/run/FLOW_ID` |

## Security

- Never commit `.env`, OpenAI keys, or Langflow API keys.
- Use HTTPS via Cloudflare for Camunda → Langflow calls.
- Stop `cloudflared` after demos.

## Official references

- [Langflow documentation](https://docs.langflow.org/)
- [Langflow Desktop](https://www.langflow.org/desktop)
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)
- [Camunda REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)
