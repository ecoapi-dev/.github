<h1 align="center">ReCost</h1>

<p align="center">
  <strong>Know what your APIs cost before they cost you.</strong>
</p>

<p align="center">
  <a href="https://recost.dev">Website</a> · <a href="https://github.com/recost-dev/extension">Extension</a> · <a href="https://github.com/recost-dev/api">API</a> · <a href="https://github.com/recost-dev/docs">Docs</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HackIllinois_2026-Stripe_Best_Web_API-635bff?style=for-the-badge" alt="HackIllinois 2026 — Stripe Best Web API" />
  <img src="https://img.shields.io/badge/license-AGPL--3.0-blue?style=for-the-badge" alt="AGPL-3.0 License" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white" alt="Cloudflare Workers" />
</p>

---

## What is ReCost?

Ever thought about how much your vibe-coded app is actually costing you? ReCost is a VS Code extension and edge-powered analysis engine that scans your codebase for API usage patterns and gives you real cost estimates, optimization insights, and sustainability metrics — token by token, one scan at a time.

Point it at any project. It finds your API calls, estimates what they cost, detects inefficient patterns, and suggests concrete improvements. No more surprise bills at the end of the month.

## How It Works

1. **Scan** — Open your project in VS Code, trigger a scan. The extension parses your codebase locally and identifies API calls across 50+ providers (OpenAI, Stripe, Twilio, AWS, and more) using 16 specialized pattern scanners.
2. **Analyze** — Structured call data is sent to the ReCost Worker running on Cloudflare's edge network. Cost estimation, pattern detection, sustainability metrics, and dependency graph construction happen at the nearest data center.
3. **Optimize** — Results appear in the extension sidebar: per-endpoint cost breakdowns, optimization suggestions with estimated savings, and an interactive dependency graph. A built-in Cost Simulator lets you project spend at scale and compare scenarios.
4. **Chat** — Ask the built-in AI assistant about your scan results. It has full context on your codebase's API footprint and can recommend specific changes. Powered by Cloudflare Workers AI (free, no API key needed) or bring your own key for OpenAI, Anthropic, Gemini, xAI, Cohere, Mistral, or Perplexity.
5. **Monitor** — Drop the Node.js or Python middleware into your app to track live API calls in production. Telemetry flows to the dashboard or VS Code extension over WebSocket, giving you real cost data alongside your static scans.

## Architecture

ReCost is split across several repos under the [`recost-dev`](https://github.com/recost-dev) organization:

| Repo | What it does |
|------|-------------|
| [`extension`](https://github.com/recost-dev/extension) | VS Code extension — codebase scanning, sidebar UI, AI chat, cost simulator, local dashboard, and sustainability stats |
| [`api`](https://github.com/recost-dev/api) | Cloudflare Worker — cost analysis engine, pattern detection, suggestion generation, telemetry ingestion, and `/chat` proxy to Workers AI |
| [`web`](https://github.com/recost-dev/web) | Dashboard frontend at [recost.dev](https://recost.dev) — project management, cost analytics, API key management |
| [`docs`](https://github.com/recost-dev/docs) | Documentation and landing page site |
| [`middleware`](https://github.com/recost-dev/middleware) | Node.js SDK — automatic HTTP interception, provider matching, and telemetry transport |
| [`middleware-python`](https://github.com/recost-dev/middleware-python) | Python SDK — same capabilities for urllib3, httpx, and aiohttp |

```
┌──────────────────────────────────┐
│         VS Code Extension        │
│                                  │
│  ┌────────┐  ┌────────────────┐  │
│  │Scanner │  │  Sidebar UI    │  │
│  │(regex) │  │  + Dashboard   │  │
│  └───┬────┘  └───────▲────────┘  │
│      │               │           │
│      │   scan data   │  results  │
│      └───────┬───────┘           │
└──────────────┼───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│     Cloudflare Worker (Edge)     │
│                                  │
│  /scans     → cost estimation    │
│  /endpoints → pattern detection  │
│  /suggest   → optimization tips  │
│  /chat      → Workers AI proxy   │
│  /telemetry → live monitoring    │
└──────────────────────────────────┘
               ▲
               │
┌──────────────┴───────────────────┐
│   Node.js / Python Middleware    │
│                                  │
│  Intercepts fetch/http/urllib3   │
│  Matches 21+ API providers       │
│  Ships telemetry via WebSocket   │
│  or HTTPS to the Worker          │
└──────────────────────────────────┘
```

## Tech Stack

- **Extension**: TypeScript, VS Code Extension API, React 18, esbuild + Vite, D3.js
- **API**: TypeScript, Hono, Cloudflare Workers, D1 (SQLite), KV, Workers AI
- **Web**: React 19, React Router v7, Vite, TanStack Query, Tailwind v4, Recharts
- **Docs**: React, Vite, Tailwind, Cloudflare Pages
- **Middleware**: TypeScript (Node.js) / Python — zero core dependencies, patches HTTP clients at runtime
- **AI Models**: Llama 3.1 8B (free via Workers AI), OpenAI, Anthropic, Gemini, xAI, Cohere, Mistral, Perplexity (BYOK)

## Getting Started

### Install the Extension

Follow the download instructions in the [`extension`](https://github.com/recost-dev/extension) README or visit our documentation at [recost.dev](https://recost.dev).

### Run a Scan

1. Open any project in VS Code
2. Click the ReCost icon in the sidebar
3. Hit **Scan** — results populate automatically

No API key required for scanning or chatting with the free Llama model. To sync results to the cloud dashboard, create an account at [recost.dev](https://recost.dev) and grab an API key.

### Add Runtime Monitoring

Drop the middleware into your app for live cost tracking:

```ts
// Node.js
import { init } from "@ecoapi/node";
init();
```

```python
# Python
from ecoapi import init
init()
```

## Features

**Cost Analysis** — Per-endpoint cost breakdowns based on detected API calls, token counts, and pricing data for 40+ providers. Know exactly where your budget goes.

**Cost Simulator** — Project your API spend at different scales (1K to 100K users). Save scenarios, compare side-by-side, export to CSV.

**Pattern Detection** — Identifies common anti-patterns like redundant calls, missing caching opportunities, unbatched requests, N+1 queries, and over-fetched responses.

**Sustainability Metrics** — Estimates electricity (kWh), water (L), and CO2 (g) footprint of your API usage, broken down by provider with AI vs non-AI separation.

**Dependency Graph** — Interactive visualization of how your API calls relate to each other across your codebase.

**AI Chat Assistant** — Context-aware chat that understands your scan results. Supports 8 providers — ReCost AI is free and requires no key.

**Live Monitoring** — Node.js and Python SDKs automatically intercept outbound HTTP calls, match them against 21+ built-in provider rules, and ship telemetry to your dashboard.

**Edge-Powered** — Analysis runs on Cloudflare Workers at the nearest data center. Fast responses regardless of where you are.

## Contributing

Contributions are welcome! Each repo has its own setup instructions:

- **Extension**: Clone [`extension`](https://github.com/recost-dev/extension), run `npm install`, press `F5` in VS Code
- **API**: Clone [`api`](https://github.com/recost-dev/api), run `npm install`, start with `npx wrangler dev`
- **Web**: Clone [`web`](https://github.com/recost-dev/web), run `npm install && npm run dev`
- **Docs**: Clone [`docs`](https://github.com/recost-dev/docs), run `npm install && npm run dev`
- **Middleware (Node)**: Clone [`middleware`](https://github.com/recost-dev/middleware), run `npm install && npm test`
- **Middleware (Python)**: Clone [`middleware-python`](https://github.com/recost-dev/middleware-python), run `pip install -e ".[dev]" && pytest`

## Awards

**Stripe Best Web API** — HackIllinois 2026

## License

All repositories are licensed under [AGPL-3.0](LICENSE).

---

<p align="center">
  Built by <a href="https://andresl.dev">Andres</a>, <a href="https://github.com/Asyboi">Aslan</a>, and <a href="https://github.com/DongYoon112">Donggyu</a>.
</p>
