# WokForge

**AI-native web automation platform — describe a workflow in plain English, watch it execute.**

- **URL:** [wokforge.wokspec.org](https://wokforge.wokspec.org)
- **Repo:** [WokSpec/WokForge](https://github.com/WokSpec/WokForge)
- **Stack:** Next.js · TypeScript · Playwright · Groq (LLM planning) · Chrome Extension
- **License:** MIT

---

## What It Is

WokForge lets you describe what you want to automate in natural language — "go to this page and extract all pricing tiers into a spreadsheet," "monitor this URL for changes and alert me" — and it converts that into a multi-step executable plan dispatched to a fleet of headless browser agents.

It's built for teams and solo operators who need repeatable web workflows without writing Playwright scripts from scratch. The Chrome Extension bridges local browser execution so sensitive workflows never have to leave the user's machine.

---

## Core Concepts

| Concept | Description |
|---|---|
| **Orchestrator (Brain)** | LLM planner (Groq / Llama 3) that converts a natural language prompt into a structured execution plan |
| **Agents (Hands)** | Specialist workers that execute plan steps — Browser, Validator, Sheets, Gmail, Slack |
| **Browser Agent** | Playwright-based headless browser; connects to a remote browser via WebSocket (Browserless / Steel) for serverless deploys |
| **Validator Agent** | Checks data quality, structure, and completeness after extraction steps |
| **Chrome Extension** | Local execution bridge — runs automation in the user's own Chrome without a remote browser |
| **Dashboard** | Real-time execution monitor with live logs and step-by-step progress |

---

## Architecture

```
User prompt
    │
Orchestrator (LLM planner — Groq)
    │  produces structured plan
    ▼
Plan Runner
    ├── BrowserAgent  (Playwright ↔ remote browser WebSocket)
    ├── ValidatorAgent
    └── [future] SheetsAgent, GmailAgent, SlackAgent
    │
Dashboard (Next.js)  ← real-time log stream
Chrome Extension     ← local execution path (no remote browser needed)
```

For serverless deployment (Vercel), Playwright cannot run native binaries. The `BROWSER_WS_ENDPOINT` env var connects the BrowserAgent to a remote browser service (Browserless.io or Steel.dev).

---

## Local Development

```bash
git clone https://github.com/WokSpec/WokForge.git
cd WokForge
npm install
cp .env.example .env.local
# Required: GROQ_API_KEY, BROWSER_WS_ENDPOINT
npm run dev     # http://localhost:3000
```

The Chrome extension lives in `chrome-extension/` and can be loaded unpacked for local testing.

---

## Project Structure

```
WokForge/
├── src/
│   ├── core/
│   │   ├── orchestrator/   LLM planner — converts prompts to plans
│   │   └── agents/         Browser, Validator, and future agents
│   ├── app/api/            REST endpoints (plan, execute, status)
│   └── components/         React UI
├── chrome-extension/       Local execution bridge (Chrome)
└── public/
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **Chopsticks** | Agent actions in Chopsticks can invoke WokForge automation recipes |
| **Eral** | Eral can act as the natural-language front-end for WokForge, interpreting intent before handing off to the orchestrator |
| **WokAPI** | Auth and subscription checks for WokForge usage tiers route through WokAPI |
| **WokSite** | WokForge linked from the WokSpec platform hub |

---

## License

[MIT](https://opensource.org/licenses/MIT)
