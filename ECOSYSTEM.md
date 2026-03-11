# WokSpec Ecosystem

An AI-native platform for people who create things. WokSpec is a family of interconnected products — each useful standalone, stronger together.

---

## Project Map

| Project | URL | What it does |
|---|---|---|
| **WokSite** | [wokspec.org](https://wokspec.org) | Platform hub, auth, marketing, user dashboard |
| **WokAPI** | [api.wokspec.org](https://api.wokspec.org) | Auth, sessions, payments, AI proxy — the shared backbone |
| **Eral** | [eral.wokspec.org](https://eral.wokspec.org) | AI layer (chat, generate, analyze) — powers every product |
| **Dilu** | [dilu.wokspec.org](https://dilu.wokspec.org) | No-code product launcher with integrations |
| **Vecto** | [vecto.wokspec.org](https://vecto.wokspec.org) | AI design studio (vectors, brand, UI, voice, code) |
| **WokGen** | [wokgen.wokspec.org](https://wokgen.wokspec.org) | AI pixel art studio (sprites, tilesets, animations) |
| **WokTool** | [tools.wokspec.org](https://tools.wokspec.org) | 80+ free browser tools — no login required |
| **WokForge** | [wokforge.wokspec.org](https://wokforge.wokspec.org) | AI-native web automation platform |
| **Chopsticks** | [chopsticks.wokspec.org](https://chopsticks.wokspec.org) | Open-source Discord bot + agent pool system |

---

## How the Pieces Connect

```
                          ┌─────────────┐
                          │   WokSite   │  ← entry point, auth flows, dashboard
                          └──────┬──────┘
                                 │ JWT (shared secret)
                          ┌──────▼──────┐
                          │   WokAPI    │  ← auth · sessions · billing · AI proxy
                          └──────┬──────┘
                                 │
                          ┌──────▼──────┐
                          │    Eral     │  ← AI layer (Workers AI / OpenAI / Groq)
                          └──────┬──────┘
                 ┌───────────────┼───────────────┐
          ┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐
          │    Vecto    │ │   WokGen    │ │    Dilu     │
          │ design AI   │ │ pixel art AI│ │ no-code     │
          └─────────────┘ └─────────────┘ └─────────────┘
                 ┌───────────────┼───────────────┐
          ┌──────▼──────┐ ┌──────▼──────┐
          │  WokForge   │ │ Chopsticks  │
          │ automation  │ │ Discord bot │
          └─────────────┘ └─────────────┘
                          ┌─────────────┐
                          │   WokTool   │  ← standalone, no auth required
                          └─────────────┘
```

---

## Shared Infrastructure

### Authentication

WokAPI is the single source of truth for user identity. It issues JWTs signed with a `JWT_SECRET` that is shared with Eral, so:
- A user logs in once at wokspec.org
- Their JWT is valid across WokAPI, Eral, and any product that verifies it
- Session revocation at WokAPI propagates to all services

### AI Routing

Products do not call AI providers directly. They call Eral (either directly or via the WokAPI proxy):

```
Product → WokAPI /v1/ai/* → Eral /v1/chat|generate|analyze → Workers AI / OpenAI / Groq
```

This means:
- Spend policy is enforced in one place (Eral)
- Model upgrades propagate to all products simultaneously
- Per-session memory is consistent across products

### Subscriptions

Stripe billing is owned by WokAPI. Products check `/v1/billing/subscription` to gate premium features. Tier names are consistent across all products (Free, Builder, Pro, Enterprise).

---

## Technology Landscape

| Domain | Technology |
|---|---|
| Edge compute | Cloudflare Workers (WokAPI, Eral, WokSite, Dilu) |
| Server-rendered apps | Next.js App Router + Vercel (Vecto, WokGen, WokTool, WokForge) |
| Edge database | Cloudflare D1 / KV (WokAPI, Eral) |
| Application database | Neon PostgreSQL via Prisma (Vecto, WokGen) |
| Bot runtime | Node.js 22 + discord.js v14 (Chopsticks) |
| Bot database | PostgreSQL + Redis (Chopsticks) |
| AI providers | CF Workers AI · OpenAI · Groq · Fal.ai · Replicate · ElevenLabs |
| Payments | Stripe |
| Email | Resend |
| Observability | Prometheus + Grafana (Chopsticks); Cloudflare Analytics (edge products) |

---

## Repositories

All repositories live under the **[WokSpec GitHub Organisation](https://github.com/WokSpec)**.

| Repo | Visibility | Primary language |
|---|---|---|
| [WokSpec/WokSite](https://github.com/WokSpec/WokSite) | Public | TypeScript |
| [WokSpec/WokAPI](https://github.com/WokSpec/WokAPI) | Public | TypeScript |
| [WokSpec/Eral](https://github.com/WokSpec/Eral) | Public | TypeScript |
| [WokSpec/Dilu](https://github.com/WokSpec/Dilu) | Public | TypeScript |
| [WokSpec/Vecto](https://github.com/WokSpec/Vecto) | Public | TypeScript |
| [WokSpec/WokGen](https://github.com/WokSpec/WokGen) | Public | TypeScript |
| [WokSpec/WokTool](https://github.com/WokSpec/WokTool) | Public | TypeScript |
| [WokSpec/WokForge](https://github.com/WokSpec/WokForge) | Public | TypeScript |
| [WokSpec/Chopsticks](https://github.com/WokSpec/Chopsticks) | Public | JavaScript |

---

## Per-Project Deep Documentation

Each project has an extended documentation folder at `docs/<project>/`:

| Project | Architecture | API | Deployment | Contributing | Security |
|---|---|---|---|---|---|
| WokSite | [→](./woksite/architecture.md) | — | [→](./woksite/deployment.md) | [→](./woksite/contributing.md) | [→](./woksite/security.md) |
| WokAPI | [→](./wokapi/architecture.md) | [→](./wokapi/api.md) | [→](./wokapi/deployment.md) | [→](./wokapi/contributing.md) | [→](./wokapi/security.md) |
| Eral | [→](./eral/architecture.md) | [→](./eral/api.md) | [→](./eral/deployment.md) | [→](./eral/contributing.md) | [→](./eral/security.md) |
| Chopsticks | [→](./chopsticks/architecture.md) | — | [→](./chopsticks/deployment.md) | [→](./chopsticks/contributing.md) | [→](./chopsticks/security.md) |
| WokGen | [→](./wokgen/architecture.md) | [→](./wokgen/api.md) | [→](./wokgen/deployment.md) | [→](./wokgen/contributing.md) | — |
| Vecto | [→](./vecto/architecture.md) | [→](./vecto/api.md) | [→](./vecto/deployment.md) | [→](./vecto/contributing.md) | — |
| Dilu | [→](./dilu/architecture.md) | — | [→](./dilu/deployment.md) | [→](./dilu/contributing.md) | — |
| WokTool | [→](./woktool/architecture.md) | — | [→](./woktool/deployment.md) | [→](./woktool/contributing.md) | — |
| WokForge | [→](./wokforge/architecture.md) | [→](./wokforge/api.md) | [→](./wokforge/deployment.md) | [→](./wokforge/contributing.md) | [→](./wokforge/security.md) |

---

## Licenses

| Scope | License |
|---|---|
| Eral, Dilu | [FSL-1.1-MIT](https://fsl.software) — source-available, converts to MIT after 2 years |
| Vecto, WokGen | [FSL-1.1-MIT](https://fsl.software) |
| WokTool, Chopsticks | [MIT](https://opensource.org/licenses/MIT) — fully open source |
| WokForge | [MIT](https://opensource.org/licenses/MIT) |
| WokSite, WokAPI | © WokSpec (proprietary platform code) |
