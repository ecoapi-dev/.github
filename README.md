<p align="center">
  <img src="https://avatars.githubusercontent.com/u/265149685?s=200&v=4" alt="EcoAPI Logo" width="120" />
</p>

<h1 align="center">EcoAPI</h1>

<p align="center">
  <strong>Know what your APIs cost before they cost you.</strong>
</p>

<p align="center">
  <a href="https://ecoapi.dev">Website</a> · <a href="https://github.com/ecoapi-dev/extension">Extension</a> · <a href="https://github.com/ecoapi-dev/api">API</a> · <a href="https://github.com/ecoapi-dev/docs">Docs</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HackIllinois_2026-Stripe_Best_Web_API-635bff?style=for-the-badge" alt="HackIllinois 2026 — Stripe Best Web API" />
  <img src="https://img.shields.io/badge/license-MIT-blue?style=for-the-badge" alt="MIT License" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white" alt="Cloudflare Workers" />
</p>

---

## What is EcoAPI?

Ever thought about how much your vibe-coded app is actually costing you? EcoAPI is a VS Code extension and edge-powered analysis engine that scans your codebase for API usage patterns and gives you real cost estimates, optimization insights, and sustainability metrics — token by token, one scan at a time.

Point it at any project. It finds your API calls, estimates what they cost, detects inefficient patterns, and suggests concrete improvements. No more surprise bills at the end of the month.

## How It Works

1. **Scan** — Open your project in VS Code, trigger a scan. The extension parses your codebase locally and identifies API calls (OpenAI, Stripe, Twilio, etc.).
2. **Analyze** — Structured call data is sent to the EcoAPI Worker running on Cloudflare's edge network. Cost estimation, pattern detection, and dependency graph construction happen at the nearest data center.
3. **Optimize** — Results appear in the extension sidebar: per-endpoint cost breakdowns, optimization suggestions, and an interactive dependency graph of your API usage.
4. **Chat** — Ask the built-in AI assistant about your scan results. It has full context on your codebase's API footprint and can recommend specific changes. Powered by Cloudflare Workers AI (free, no API key needed) or bring your own OpenAI key for GPT models.

## Architecture

EcoAPI is split across three repos under the [`ecoapi-dev`](https://github.com/ecoapi-dev) organization:

| Repo | What it does |
|------|-------------|
| [`extension`](https://github.com/ecoapi-dev/extension) | VS Code extension — codebase parsing, sidebar UI, scan results rendering, AI chat, and local dashboard |
| [`api`](https://github.com/ecoapi-dev/api) | Cloudflare Worker — cost analysis engine, pattern detection, suggestion generation, and `/chat` proxy to Workers AI |
| [`docs`](https://github.com/ecoapi-dev/docs) | Documentation site at [ecoapi.dev](https://ecoapi.dev) |

```
┌──────────────────────────────────┐
│         VS Code Extension        │
│                                  │
│  ┌────────┐  ┌────────────────┐  │
│  │ Parser │  │  Sidebar UI    │  │
│  │  (AST) │  │  + Dashboard   │  │
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
│  /analyze   → cost estimation    │
│  /results   → KV read           │
│  /suggest   → optimization tips  │
│  /chat      → Workers AI proxy   │
└──────────────────────────────────┘
```

## Tech Stack

- **Extension**: TypeScript, VS Code Extension API, Webview panels
- **API**: TypeScript, Hono, Cloudflare Workers, Workers AI, D1/KV
- **Docs**: TypeScript, deployed on Cloudflare Pages
- **AI Models**: Llama 3.1 8B (free via Workers AI), OpenAI GPT (BYOK)

## Getting Started

### Install the Extension

Search for **EcoAPI** in the VS Code Marketplace, or install from the command line:

```bash
code --install-extension ecoapi.ecoapi
```

### Run a Scan

1. Open any project in VS Code
2. Click the EcoAPI icon in the sidebar
3. Hit **Scan** — results populate automatically

No API key required for scanning or chatting with the free Llama model. To use OpenAI models for the chat assistant, enter your key when prompted.

## Features

**Cost Analysis** — Per-endpoint cost breakdowns based on detected API calls, token counts, and pricing data. Know exactly where your budget goes.

**Pattern Detection** — Identifies common anti-patterns like redundant calls, missing caching opportunities, unbatched requests, and over-fetched responses.

**Dependency Graph** — Interactive visualization of how your API calls relate to each other across your codebase.

**AI Chat Assistant** — Context-aware chat that understands your scan results. Ask it things like "which endpoint is costing me the most?" or "how can I reduce my OpenAI spend?"

**Edge-Powered** — Analysis runs on Cloudflare Workers at the nearest data center. Fast responses regardless of where you are.

## Contributing

Contributions are welcome! Each repo has its own setup instructions:

- **Extension**: Clone [`extension`](https://github.com/ecoapi-dev/extension), run `npm install`, press `F5` in VS Code to launch the development host
- **API**: Clone [`api`](https://github.com/ecoapi-dev/api), run `npm install`, start the local dev server with `npx wrangler dev`
- **Docs**: Clone [`docs`](https://github.com/ecoapi-dev/docs), run `npm install && npm run dev`

## Awards

🏆 **Stripe Best Web API** — HackIllinois 2026

## License

All repositories are licensed under [MIT](LICENSE).

---

<p align="center">
  Built by <a href="https://andresl.dev">Andres</a>, <a href="https://github.com/Asyboi">Aslan</a>, and <a href="https://github.com/DongYoon112">Donggyu</a>.
</p>
