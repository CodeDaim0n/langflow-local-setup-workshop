# Langflow Local Setup Guide — Windows and macOS

**Purpose:** Run [Langflow](https://www.langflow.org/) on your laptop, add one AI model key, test a first flow, and let [Camunda](https://camunda.com/platform/) call that local flow through [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/).

**Recommended setup for a demo:**

```text
Camunda SaaS / Camunda 8
        |
        | HTTPS + Langflow API key
        v
Cloudflare Tunnel public HTTPS address
        |
        v
Langflow on your laptop
http://localhost:7860
```

> Camunda runs in [Camunda SaaS / Camunda 8](https://console.camunda.io/).

> **Workshop context:** Camunda BPMN, integrations, and the business loan flow guide are in the separate [business-loan-onboarding-workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop) repository. Export flow JSON to the [`flows/`](../flows/) folder in this repo.

> This is suitable for development and a controlled demo. Keep your laptop on, keep Langflow running, and do not treat a local tunnel as a production deployment.

---

# 1. What you need

| Item | Why you need it | Needed immediately? |
|---|---|---|
| Windows or macOS computer | Runs Langflow | Yes |
| Internet connection | Downloads software and calls cloud AI models | Yes |
| OpenAI API key, or another supported model-provider key | Lets your Langflow agent answer questions | Yes |
| Langflow API key | Lets Camunda authenticate when calling a Langflow flow | When integrating Camunda |
| [Cloudflare account](https://dash.cloudflare.com/sign-up) | Provides a public HTTPS route from Camunda to your laptop | When integrating Camunda |
| [`cloudflared`](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/) | Cloudflare Tunnel client installed on your laptop | When integrating Camunda |

> Keep all keys out of prompts, screenshots, GitHub, email, Teams, BPMN XML, and shared documents.

---

# 2. Route A — Recommended for beginners: Langflow Desktop

[Langflow Desktop](https://www.langflow.org/desktop) is the simplest route because it manages the local Python environment and dependencies for you.

## Step 1 — Install Langflow Desktop

1. Download **Langflow Desktop** for your operating system from the [official Langflow Desktop page](https://www.langflow.org/desktop) (see also the [installation guide](https://docs.langflow.org/get-started-installation)).
2. Run the installer.
3. Open **Langflow**.

Langflow normally opens locally at:

```text
http://localhost:7860
```

## Step 2 — Confirm it is working

In a browser, open:

```text
http://localhost:7860
```

You should see the Langflow home screen.

## Step 3 — Create an OpenAI API key

1. Sign in to the [OpenAI Platform](https://platform.openai.com/) account used for development.
2. Open [**API Keys**](https://platform.openai.com/api-keys).
3. Create a new secret key.
4. Name it clearly, for example:

```text
langflow-local-demo
```

5. Copy it immediately and save it in an approved password manager.

> This is a **model-provider key**. It is not the Langflow API key used by Camunda later.

## Step 4 — Add the provider key in Langflow

1. In Langflow, select your **profile icon**.
2. Select **Settings**.
3. Open **Model Providers**.
4. Select **OpenAI**.
5. Paste the key into **API Key**.
6. Save.
7. Enable the text model you intend to use.

You only need one provider to start. You can later use another provider supported by your installed Langflow version.

## Step 5 — Test a first flow

Follow the [Langflow Quickstart](https://docs.langflow.org/get-started-quickstart) if you want a guided walkthrough.

1. Click **New Flow**.
2. Choose **Simple Agent**.
3. In the Agent component, select the enabled model.
4. Click **Playground**.
5. Enter:

```text
Explain in one sentence what a business loan application is.
```

A useful response confirms that Langflow can reach your model provider.

## Step 6 — Save and export

Rename the flow clearly, for example:

```text
01 - First Agent Test
```

Export the flow JSON before major changes and store it in a project folder.

---

# 3. Route B — Python virtual environment: Windows and macOS

Use this route when you want a repeatable developer setup, custom packages, or a local project folder.

## 3.1 Install prerequisites

Install:

- [Python 3.10–3.14](https://www.python.org/downloads/)
- [`uv` package manager](https://docs.astral.sh/uv/getting-started/installation/)
- A modern browser

See the [Langflow OSS installation guide](https://docs.langflow.org/get-started-installation#install-and-run-the-langflow-oss-python-package) for the full procedure.

## 3.2 Windows setup

Open **PowerShell**:

```powershell
mkdir C:\langflow
cd C:\langflow
uv venv .venv
```

Activate the environment:

```powershell
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, run this once in the same window:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Then activate again:

```powershell
.\.venv\Scripts\Activate.ps1
```

Install and run Langflow:

```powershell
uv pip install langflow
langflow run
```

Open:

```text
http://localhost:7860
```

### Every time you restart Windows

```powershell
cd C:\langflow
.\.venv\Scripts\Activate.ps1
langflow run
```

---

## 3.3 macOS setup

Open **Terminal**:

```bash
mkdir -p ~/langflow
cd ~/langflow
uv venv .venv
source .venv/bin/activate
uv pip install langflow
langflow run
```

Open:

```text
http://localhost:7860
```

### Every time you restart your Mac

```bash
cd ~/langflow
source .venv/bin/activate
langflow run
```

### Apple Silicon note

On Apple Silicon Macs, use the normal macOS installer for Python and the standard `uv` install. You do not need a separate Langflow command merely because your Mac uses an M-series chip.

---

# 4. Keys: what they are and where to store them

## A. Model-provider key — needed first

This lets Langflow call an AI model.

Example:

```text
OPENAI_API_KEY
```

For the first test, add it in:

```text
Langflow → Profile → Settings → Model Providers → OpenAI
```

Use it for: chat, extraction, document analysis, embeddings, vision, or other model tasks.

---

## B. Langflow API key — needed when Camunda calls Langflow

Create this only after the flow works in the Playground. See [Langflow API keys and authentication](https://docs.langflow.org/api-keys-and-authentication).

1. In Langflow, go to **Profile → Settings**.
2. Open **Langflow API Keys**.
3. Select **Add New**.
4. Name the key:

```text
camunda-local-demo
```

5. Copy it once and save it in your password manager.

Camunda sends this key to Langflow as:

```http
x-api-key: YOUR_LANGFLOW_API_KEY
```

Do not use your OpenAI key in Camunda. Camunda needs the **Langflow API key**.

---

# 5. Recommended local key management

## Option 1 — Beginner route: Langflow Settings

Use Langflow’s provider settings or global variables for your first proof of concept.

## Option 2 — Repeatable route: `.env` file

Create a `.env` file in your project folder. For details, see [Langflow global variables](https://docs.langflow.org/configuration-global-variables) and [environment variables](https://docs.langflow.org/environment-variables).

### Windows

```text
C:\langflow\.env
```

### macOS

```text
~/langflow/.env
```

Example:

```dotenv
OPENAI_API_KEY=replace_with_your_key
LANGFLOW_VARIABLES_TO_GET_FROM_ENVIRONMENT=OPENAI_API_KEY
LANGFLOW_FALLBACK_TO_ENV_VAR=True
```

Start Langflow with the `.env` file.

### Windows

```powershell
langflow run --env-file .env
```

### macOS

```bash
langflow run --env-file .env
```

Create a `.gitignore` file:

```gitignore
.env
.venv/
__pycache__/
```

Never commit `.env` to Git.

---


---

# 6A. Optional: use Ollama + Qwen for zero-cost local API calls

This route is optional. Use it when attendees want to avoid cloud model-provider charges or when you want to show a local AI setup.

## When to use OpenAI vs Ollama

| Choice | Best for | Trade-off |
|---|---|---|
| OpenAI API key | Reliable live demos, better speed, stronger quality, fewer machine issues | Requires API billing and internet |
| Ollama + Qwen | Local zero-cost API calls, privacy-friendly experiments, offline-style demos | Larger downloads, slower responses, hardware dependent |

Recommended workshop position:

```text
OpenAI is the reliable presenter path.
Ollama is the optional local zero-cost API path.
```

## Install Ollama

### Windows

1. Download Ollama for Windows from the [official Ollama download page](https://ollama.com/download/windows).
2. Run the installer.
3. Open a new PowerShell window.
4. Verify:

```powershell
ollama --version
```

### macOS

Install Ollama using the [official macOS app](https://ollama.com/download/mac). See the [Ollama documentation](https://docs.ollama.com/) for other install options.

Verify:

```bash
ollama --version
```

Ollama normally exposes a local model API at:

```text
http://localhost:11434
```

## Pull Qwen models

Start small. Larger models may be better, but they take longer to download and can be slower on attendee laptops. Browse available tags in the [Ollama library](https://ollama.com/library) (for example, [qwen3](https://ollama.com/library/qwen3) and [qwen3-vl](https://ollama.com/library/qwen3-vl)).

### Text model

```bash
ollama run qwen3:4b
```

Stronger option:

```bash
ollama run qwen3:8b
```

### Vision model

Use this when you want local image, screenshot, or document-image analysis.

```bash
ollama run qwen3-vl:4b
```

Stronger option:

```bash
ollama run qwen3-vl:8b
```

## Use Ollama in Langflow

See the [Langflow Ollama bundle documentation](https://docs.langflow.org/bundles-ollama).

1. Make sure Ollama is running.
2. In Langflow, add an **Ollama** model component.
3. Set the Ollama base URL to:

```text
http://localhost:11434
```

4. Select the model you pulled, for example:

```text
qwen3:4b
```

5. Test in Playground before connecting Camunda.

For agents that need tools, make sure the selected local model supports tool calling and verify the behaviour in Langflow Playground.

## Latency warning for attendees

Local models are not “free compute”. They use the learner’s CPU/GPU, memory, battery, disk space and time.

For live delivery:

- Pull the model before the event.
- Use smaller models first.
- Keep OpenAI configured on the presenter machine as the fallback.
- Do not debug Langflow, Ollama, Cloudflare and Camunda at the same time. Test local model behaviour first.

# 7. Expose Langflow to Camunda using Cloudflare Tunnel

## Before you start

You must have:

- Langflow working at `http://localhost:7860`
- A completed flow that works in Langflow Playground
- A Langflow API key
- A [Cloudflare account](https://dash.cloudflare.com/)
- Camunda access that lets you configure an outbound [REST Connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)

[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/) creates an **outbound connection from your laptop to Cloudflare**. You do not need to open an inbound firewall port or configure router port forwarding.

---

## Option A — Temporary demo tunnel: quickest setup

Use this for a short, supervised demo. It creates a temporary public URL via [Cloudflare Quick Tunnels](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/do-more-with-tunnels/trycloudflare/). The address changes whenever you stop and restart the tunnel.

### Step 1 — Install `cloudflared`

### Windows

Use the official [Cloudflare `cloudflared` download and installation instructions](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/). After installation, open a new PowerShell window and verify:

```powershell
cloudflared --version
```

### macOS

Install with [Homebrew](https://formulae.brew.sh/formula/cloudflared):

```bash
brew install cloudflared
```

Verify:

```bash
cloudflared --version
```

### Step 2 — Start Langflow

Ensure Langflow is running first:

```text
http://localhost:7860
```

### Step 3 — Start a quick tunnel

Open a second terminal window.

#### Windows PowerShell

```powershell
cloudflared tunnel --url http://localhost:7860
```

#### macOS Terminal

```bash
cloudflared tunnel --url http://localhost:7860
```

Cloudflare displays a temporary HTTPS address similar to:

```text
https://random-words.trycloudflare.com
```

Copy that HTTPS address.

### Step 4 — Verify the public address

Open the copied address in a browser. The Langflow page should appear.

> Do not share this URL publicly. Anyone who knows it can reach the Langflow interface. The API call still requires the Langflow API key, but the UI is not an appropriate public service.

---

## Option B — Recommended for repeated demos: named Cloudflare Tunnel

Use this if the demo will run more than once or you want a stable URL such as:

```text
https://langflow-demo.yourdomain.com
```

This requires a domain managed in Cloudflare. Follow the [Create a tunnel (dashboard)](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/get-started/create-remote-tunnel/) guide.

### Step 1 — In Cloudflare, create a tunnel

1. Sign in to [Cloudflare](https://dash.cloudflare.com/).
2. Go to **Networking → Tunnels**.
3. Select **Create a tunnel**.
4. Name it:

```text
langflow-local-demo
```

5. Choose your operating system.
6. Copy and run the connector installation command Cloudflare provides.

### Step 2 — Add a public hostname

In the same tunnel configuration:

1. Add a **Public Hostname**.
2. Choose a subdomain, for example:

```text
langflow-demo.yourdomain.com
```

3. Set **Service type** to `HTTP`.
4. Set **URL** to:

```text
localhost:7860
```

5. Save the hostname.
6. Start the tunnel connector on your laptop using the command Cloudflare supplied.

### Step 3 — Verify it

Open:

```text
https://langflow-demo.yourdomain.com
```

You should reach Langflow.

> A named tunnel is more stable than a temporary `trycloudflare.com` address. It is still a laptop-hosted demo, not a production service.

---

# 8. Configure the Camunda REST Connector

Use a **REST Connector** in your Camunda BPMN process. See the [REST connector documentation](https://docs.camunda.io/docs/components/connectors/protocol/rest/) and [using secrets in connectors](https://docs.camunda.io/docs/components/connectors/use-connectors/#using-secrets).

## Step 1 — Copy the Langflow flow ID

1. Open the completed flow in Langflow.
2. Open the **API access** area.
3. Copy the flow ID.
4. Use the Langflow run endpoint format (see [Flow trigger endpoints](https://docs.langflow.org/api-flows-run)):

```text
https://YOUR-CLOUDFLARE-ADDRESS/api/v1/run/YOUR_FLOW_ID?stream=false
```

For example:

```text
https://random-words.trycloudflare.com/api/v1/run/12345678-abcd-1234-abcd-1234567890ab?stream=false
```

## Step 2 — Store the Langflow API key in Camunda Secrets

In [Camunda Console](https://console.camunda.io/):

1. Open your cluster.
2. Open **Secrets** (see [Connector secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)).
3. Create a secret called:

```text
LANGFLOW_API_KEY
```

4. Paste the Langflow API key as the secret value.

Do not hard-code it in BPMN.

## Step 3 — Configure the REST Connector

Use these values.

| REST Connector field | Value |
|---|---|
| Method | `POST` |
| URL | Your Cloudflare public URL plus `/api/v1/run/FLOW_ID?stream=false` |
| Authentication | `No Authentication` |
| Headers | `Content-Type: application/json` and `x-api-key: {{secrets.LANGFLOW_API_KEY}}` |
| Request body | JSON shown below |

### Headers as a FEEL expression

```feel
= {
  "Content-Type": "application/json",
  "x-api-key": "{{secrets.LANGFLOW_API_KEY}}"
}
```

### Request body example

This sends the business application text held in the Camunda variable `applicationSummary`.

```feel
= {
  "inputs": {
    "text": applicationSummary
  },
  "tweaks": {}
}
```

> The exact input key may differ if you changed the flow’s input component. Use the API example generated by Langflow for the final shape. See [Get started with the Langflow API](https://docs.langflow.org/api-reference-api-examples).

## Step 4 — Store the response

Set the REST Connector result variable to:

```text
langflowResponse
```

Then inspect the output in Camunda during a test run. Map the relevant response field into later process variables only after you see the real response structure.

---

# 9. Minimal end-to-end test

1. Start Langflow.
2. Confirm the flow works in Playground.
3. Start Cloudflare Tunnel.
4. Open the Cloudflare public HTTPS URL in a browser.
5. In Camunda, run the REST Connector task.
6. Confirm the task returns HTTP `200`.
7. Inspect `langflowResponse`.
8. Stop the tunnel after the demo.

---

# 10. Security rules for the demo

- Use the **Langflow API key** for every Camunda-to-Langflow call.
- Store that key in **Camunda Secrets**, not BPMN fields or process variables.
- Use HTTPS through Cloudflare; never call `http://localhost:7860` from Camunda SaaS.
- Do not publish screenshots containing tunnel URLs, API keys, flow IDs, or payloads with personal data.
- Stop `cloudflared` after the session with `Ctrl + C`.
- Rotate the Langflow API key if it is accidentally exposed.
- Never expose a local Langflow instance as a long-running production solution. Deploy Langflow to a controlled, authenticated environment for production.

---

# 11. Common issues

## Camunda receives “connection refused” or timeout

Check:

1. Langflow is still running on port `7860`.
2. `cloudflared` is still running.
3. The Cloudflare URL is current. Quick-tunnel URLs change after restart.
4. The URL contains `/api/v1/run/FLOW_ID?stream=false`.
5. Your corporate network allows outbound connection from the Camunda environment to the Cloudflare address.

## Camunda receives 401 or 403

Check that:

- You created a **Langflow API key**, not an OpenAI key.
- The header name is exactly `x-api-key`.
- The Camunda secret name and reference match.
- The secret value has no extra spaces or quote marks.

## Camunda receives 404

Usually the flow ID is incorrect. Copy it again from Langflow API access.

## Camunda receives 422 or 400

The request-body JSON does not match the flow’s expected input. Open the flow’s API example in Langflow and copy its structure. See [Flow trigger endpoints](https://docs.langflow.org/api-flows-run).

## The tunnel works in a browser but fails from Camunda

The most common cause is an incorrect REST Connector URL, header expression, or request body. Test the exact URL and key first with the API example generated by Langflow. See the [REST connector documentation](https://docs.camunda.io/docs/components/connectors/protocol/rest/).

---

# 12. First-day checklist

- [ ] Langflow opens locally at `http://localhost:7860`.
- [ ] A model-provider key is configured, or Ollama is installed and a Qwen model is pulled.
- [ ] A Simple Agent answers in Playground.
- [ ] Optional Ollama route has been tested locally before tunnelling.
- [ ] The flow is named and exported.
- [ ] A Langflow API key is created.
- [ ] `cloudflared` is installed.
- [ ] A Cloudflare public HTTPS URL reaches the local Langflow instance.
- [ ] The Langflow API key is stored in Camunda Secrets.
- [ ] Camunda REST Connector receives a successful Langflow response.
- [ ] The tunnel is stopped after the demo.

---

# Official references

## Langflow

- [Langflow documentation](https://docs.langflow.org/)
- [Install Langflow](https://docs.langflow.org/get-started-installation)
- [Langflow Desktop download](https://www.langflow.org/desktop)
- [Quickstart](https://docs.langflow.org/get-started-quickstart)
- [API keys and authentication](https://docs.langflow.org/api-keys-and-authentication)
- [Get started with the Langflow API](https://docs.langflow.org/api-reference-api-examples)
- [Flow trigger endpoints](https://docs.langflow.org/api-flows-run)
- [Global variables and `.env` configuration](https://docs.langflow.org/configuration-global-variables)
- [Environment variables](https://docs.langflow.org/environment-variables)
- [Ollama bundle](https://docs.langflow.org/bundles-ollama)

## Cloudflare Tunnel

- [Cloudflare Tunnel overview](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)
- [Quick Tunnels (TryCloudflare)](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/do-more-with-tunnels/trycloudflare/)
- [Create a named tunnel (dashboard)](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/get-started/create-remote-tunnel/)
- [`cloudflared` downloads](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/)

## Camunda

- [Camunda 8 platform](https://camunda.com/platform/)
- [Camunda Console](https://console.camunda.io/)
- [REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)
- [Connector secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)
- [Using secrets in connectors](https://docs.camunda.io/docs/components/connectors/use-connectors/#using-secrets)

## Model providers

- [OpenAI API keys](https://platform.openai.com/api-keys)
- [Ollama download (Windows)](https://ollama.com/download/windows)
- [Ollama download (macOS)](https://ollama.com/download/mac)
- [Ollama documentation](https://docs.ollama.com/)
- [Ollama model library](https://ollama.com/library)

## Tools

- [Python downloads](https://www.python.org/downloads/)
- [`uv` installation](https://docs.astral.sh/uv/getting-started/installation/)
- [Homebrew `cloudflared` formula](https://formulae.brew.sh/formula/cloudflared)
