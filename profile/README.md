<h1 align="center">ReCost</h1>

<p align="center">
  <strong>Know what your APIs cost before they cost you.</strong>
</p>

<p align="center">
  <a href="https://recost.dev">Website</a> · 
  <a href="https://github.com/recost-dev/extension">Extension</a> · 
  <a href="https://github.com/recost-dev/api">API</a> · 
  <a href="https://github.com/recost-dev/docs">Docs</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HackIllinois_2026-Stripe_Best_Web_API-635bff?style=for-the-badge" />
  <img src="https://img.shields.io/badge/license-AGPL--3.0-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white" />
</p>

---

## What is ReCost?

ReCost shows you exactly what your APIs cost — before production does.

* Scan your codebase → detect API usage
* Estimate cost per endpoint → real numbers
* Catch inefficiencies → N+1, loops, missing cache
* Track live usage → runtime telemetry
* Simulate spend → before you scale

---

## How it works

```
Code → Scan → Analyze → Optimize → Monitor
```

* **Scan** — VS Code extension parses your code (AST + patterns)
* **Analyze** — Edge API computes cost, risk, and suggestions
* **Optimize** — Dashboard + extension show savings opportunities
* **Monitor** — SDKs track real API usage in production

---

## Stack

* **Extension** — TypeScript, React, tree-sitter
* **API** — Cloudflare Workers, Hono, D1
* **Web** — React, TanStack Query, Tailwind
* **SDKs** — Node + Python (runtime interception)
* **AI** — Workers AI + BYOK providers

---

## Repos

* `extension` — code scanning + UI
* `api` — cost engine + analysis
* `web` — dashboard
* `@recost/node` — Node SDK
* `recost` — Python SDK
* `automated-parser` — scheduled ingestion
* `docs` — site + docs

---

## Quick start

**Scan your code**

1. Install the VS Code extension
2. Open a project
3. Click **Scan**

**Track production usage**

```ts
import { init } from "@recost/node";
init();
```

```python
from recost import init
init()
```

---

## Why ReCost?

APIs (especially AI) make costs unpredictable.

ReCost makes them:

* visible
* predictable
* optimizable

---

## License

AGPL-3.0

---

<p align="center">
  Built by Andres Lopez, Aslan Wang, and Donggyu Yoon.
</p>

---
