# Eral

**The AI backbone for WokSpec. Ships as a Cloudflare Worker API, an embeddable widget, and a browser extension.**

- **URL:** [eral.wokspec.org](https://eral.wokspec.org)
- **Repo:** [WokSpec/Eral](https://github.com/WokSpec/Eral)
- **Stack:** Cloudflare Workers · Hono · Cloudflare Workers AI · Cloudflare KV (memory)
- **License:** FSL-1.1-MIT

---

## What It Is

Eral is the shared AI layer that powers every WokSpec product. Rather than each product integrating its own LLM calls, they all route through Eral — getting a consistent API, persistent session memory, spend controls, and a unified chat/generate/analyze surface.

It ships in three forms:
- **API** — a Cloudflare Worker with REST endpoints for chat, generation, and analysis
- **Widget** — a drop-in `<script>` tag that embeds a floating AI panel in any page (Shadow DOM, no style leakage)
- **Browser Extension** — Plasmo-based Chrome/Firefox/Edge extension that adds Eral to any third-party site

---

## API Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/health` | Health check |
| `GET` | `/v1/status` | Provider info, spend policy, model variants |
| `POST` | `/v1/chat` | Chat with persistent session memory |
| `GET` | `/v1/chat/sessions` | List sessions |
| `GET` | `/v1/chat/:sessionId` | Session history |
| `DELETE` | `/v1/chat/:sessionId` | Clear session |
| `POST` | `/v1/generate` | Content generation + transforms (improve, rewrite, expand, shorten) |
| `POST` | `/v1/analyze` | Content analysis (summarize, review, extract) |

All endpoints require `Authorization: Bearer <jwt>` — the same JWT issued by WokAPI.

---

## AI Providers

| Provider | Role |
|---|---|
| Cloudflare Workers AI (Llama 3.3 70B) | Default — free, runs at edge |
| OpenAI GPT-4o | Optional paid override |
| Groq (Llama 3.3 70B) | High-speed inference path for WokGen / Vecto |

Eral defaults to `free-only` mode — stays on Workers AI even if an `OPENAI_API_KEY` is present. Upgrade to `paid-fallback` or `paid-primary` explicitly.

---

## Integration Context

Every main endpoint accepts an optional `integration` field so the calling product can tell Eral where it's running and what the user is doing — enabling context-aware responses without Eral having to know about each product's data model.

```json
{
  "product": "wokgen",
  "integration": {
    "name": "WokGen Pixel Studio",
    "kind": "webapp",
    "url": "https://wokgen.wokspec.org/pixel",
    "capabilities": ["chat", "generate"],
    "instructions": "User is generating pixel art. Help with style and prompt guidance."
  }
}
```

---

## Deploying the Widget

```html
<script
  src="https://eral.wokspec.org/widget.js"
  data-eral-key="eral_..."
  data-eral-name="Eral"
  data-eral-product="your-product"
  data-eral-quality="best"
  data-eral-page-context="true"
></script>
```

---

## Local Development

```bash
cd apps/api
npm install
npm run dev       # wrangler dev on :8788

cd apps/extension
npm install
npm run dev       # loads unpacked in Chrome
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **WokAPI** | Shares `JWT_SECRET`; WokAPI proxies `/v1/ai/*` calls to Eral |
| **WokSite** | Eral widget embedded on the main marketing site |
| **WokGen** | Eral companion in every WokGen page; can trigger generation jobs |
| **Vecto** | Eral companion provides content and code assistance across all studios |
| **Dilu** | Eral included in Dilu's Builder tier for full AI assistance |
| **Chopsticks** | `/ai chat` and future assistant features route through Eral's API |
| **WokForge** | Planned: Eral as natural-language front-end for automation intent |

---

## License

[FSL-1.1-MIT](https://fsl.software) — converts to MIT two years after publication.
