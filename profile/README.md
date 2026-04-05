<div align="center">

# ReCost
Know what your APIs cost before they cost you.
Website · Extension · API · Docs

</div>

&nbsp;
&nbsp;

## What is ReCost?
ReCost shows you exactly what your APIs cost — before production does.

- Scan your codebase → detect API usage
- Estimate cost per endpoint → real numbers
- Catch inefficiencies → N+1, loops, missing cache
- Track live usage → runtime telemetry
- Simulate spend → before you scale

## How it works

```
Code → Scan → Analyze → Optimize → Monitor
```

- **Scan** — VS Code extension parses your code (AST + patterns) using web-tree-sitter for JS/TS/Python
- **Analyze** — Edge API computes cost, risk, sustainability stats, and suggestions
- **Optimize** — Dashboard + extension show savings opportunities, Cost Simulator projects spend at scale
- **Monitor** — SDKs intercept real API calls at runtime and ship telemetry to the dashboard or extension

## Stack

| Repo | Tech |
|---|---|
| **extension** | TypeScript, React 18, web-tree-sitter (WASM), Vite, TanStack Query v5, Tailwind v4, Radix UI |
| **api** | Cloudflare Workers, Hono, D1 (SQLite), KV, TypeScript |
| **web** | React 19, React Router v7, Vite, TanStack Query v5, Tailwind v4, shadcn/ui, Framer Motion, Recharts |
| **middleware-node** | TypeScript, Node.js ≥ 18, patches `fetch`/`http`/`https`, Express + Fastify adapters |
| **middleware-python** | Python ≥ 3.9 (no core deps), patches urllib3/httpx/aiohttp, FastAPI + Flask adapters |
| **automated-parser** | Node.js, GitHub Actions (daily cron) |

## Repos

- `extension` — VS Code extension: code scanning, AI chat, Cost Simulator sidebar
- `api` — cost engine, analysis, auth, telemetry ingest
- `web` — dashboard (projects, live telemetry, analytics, docs)
- `middleware-node` — Node.js SDK: runtime HTTP interception
- `middleware-python` — Python SDK: runtime HTTP interception
- `automated-parser` — scheduled GitHub repo ingestion

## Quick start

### Scan your code

Install the VS Code extension, open a project, and click **Scan**. Or build from source:

```bash
cd extension
bash scripts/build-vsix.sh
code --install-extension recost-api-analyzer-*.vsix
```

### Track production usage

**Node.js**

```bash
npm install @recost-dev/node
```

```ts
import { init } from "@recost-dev/node";

// Local mode — streams to VS Code extension, no key needed
init();

// Cloud mode — sends to recost.dev dashboard
init({
  apiKey: process.env.RECOST_API_KEY,
  projectId: process.env.RECOST_PROJECT_ID,
});
```

Express and Fastify middleware adapters are included.

**Python**

```bash
pip install recost
# pip install recost[fastapi]  # FastAPI/Starlette middleware
# pip install recost[flask]    # Flask extension
```

```python
from recost import init

# Local mode — streams to VS Code extension, no key needed
init()

# Cloud mode — sends to recost.dev dashboard
from recost import init, RecostConfig
init(RecostConfig(
    api_key=os.environ["RECOST_API_KEY"],
    project_id=os.environ["RECOST_PROJECT_ID"],
))
```

## What gets tracked

The SDKs capture metadata only — URL (query params stripped), method, status, latency, byte sizes, matched provider, and estimated cost. Headers and body content are never captured.

Supported providers out of the box: OpenAI, Anthropic, Stripe, Twilio, SendGrid, Pinecone, AWS, Google Cloud, GitHub, and more.

## AI Chat

The extension includes a built-in AI chat powered by **ReCost AI** (free, no key required). You can also bring your own key for OpenAI, Anthropic, Gemini, xAI, Cohere, Mistral, or Perplexity.

## Why ReCost?

APIs (especially AI) make costs unpredictable.

ReCost makes them:

- visible
- predictable
- optimizable

##

Built by Andres Lopez, Aslan Wang, and Donggyu Yoon.
