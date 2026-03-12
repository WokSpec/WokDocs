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
| **WokPost** | [wokpost.wokspec.org](https://wokpost.wokspec.org) | AI-curated news aggregator and editorial platform |
| **WokTool** | [tools.wokspec.org](https://tools.wokspec.org) | 80+ free browser tools — no login required |
| **WokForge** | [wokforge.wokspec.org](https://wokforge.wokspec.org) | AI-native web automation platform |
| **Chopsticks** | [chopsticks.wokspec.org](https://chopsticks.wokspec.org) | Open-source Discord bot + agent pool system |
| **GatheRice** | [gatherice.wokspec.org](https://gatherice.wokspec.org) | Minimalist high-performance rice-gathering idle game |

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
          ┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐
          │  WokForge   │ │ Chopsticks  │ │   WokPost   │
          │ automation  │ │ Discord bot │ │ AI news     │
          └─────────────┘ └─────────────┘ └─────────────┘
          ┌─────────────┐ ┌─────────────┐
          │   WokTool   │ │  GatheRice  │  ← standalone products
          └─────────────┘ └─────────────┘
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
| [WokSpec/wokpost](https://github.com/WokSpec/wokpost) | Public | TypeScript |
| [WokSpec/GatheRice](https://github.com/WokSpec/GatheRice) | Public | TypeScript |

---

## Per-Project Deep Documentation

Each project has an extended documentation folder at `docs/<project>/`:

| Project | Architecture | API | Deployment | Contributing | Security |
|---|---|---|---|---|---|
| WokSite | [→](../../WokSite/docs/architecture.md) | — | [→](../../WokSite/docs/deployment.md) | [→](../../WokSite/docs/contributing.md) | [→](../../WokSite/docs/security.md) |
| WokAPI | [→](../../WokAPI/docs/architecture.md) | [→](../../WokAPI/docs/api.md) | [→](../../WokAPI/docs/deployment.md) | [→](../../WokAPI/docs/contributing.md) | [→](../../WokAPI/docs/security.md) |
| Eral | [→](../../Eral/docs/architecture.md) | [→](../../Eral/docs/api.md) | [→](../../Eral/docs/deployment.md) | [→](../../Eral/docs/contributing.md) | [→](../../Eral/docs/security.md) |
| Chopsticks | [→](../../Chopsticks/docs/architecture.md) | — | [→](../../Chopsticks/docs/deployment.md) | [→](../../Chopsticks/docs/contributing.md) | [→](../../Chopsticks/docs/security.md) |
| WokGen | [→](../../WokGen/docs/architecture.md) | [→](../../WokGen/docs/api.md) | [→](../../WokGen/docs/deployment.md) | [→](../../WokGen/docs/contributing.md) | — |
| Vecto | [→](../../Vecto/docs/architecture.md) | [→](../../Vecto/docs/api.md) | [→](../../Vecto/docs/deployment.md) | [→](../../Vecto/docs/contributing.md) | — |
| Dilu | [→](../../Dilu/docs/architecture.md) | — | [→](../../Dilu/docs/deployment.md) | [→](../../Dilu/docs/contributing.md) | — |
| WokTool | [→](../../WokTool/docs/architecture.md) | — | [→](../../WokTool/docs/deployment.md) | [→](../../WokTool/docs/contributing.md) | — |
| WokForge | [→](../../WokForge/docs/architecture.md) | [→](../../WokForge/docs/api.md) | [→](../../WokForge/docs/deployment.md) | [→](../../WokForge/docs/contributing.md) | [→](../../WokForge/docs/security.md) |
| WokPost | [→](../../wokpost/docs/architecture.md) | — | [→](../../wokpost/docs/deployment.md) | — | — |
| GatheRice | [→](../../GatheRice/docs/architecture.md) | — | — | — | — |

---

## Licenses

| Scope | License |
|---|---|
| Eral, Dilu | [FSL-1.1-MIT](https://fsl.software) — source-available, converts to MIT after 2 years |
| Vecto, WokGen | [FSL-1.1-MIT](https://fsl.software) |
| WokTool, Chopsticks | [MIT](https://opensource.org/licenses/MIT) — fully open source |
| WokForge | [MIT](https://opensource.org/licenses/MIT) |
| WokSite, WokAPI | © WokSpec (proprietary platform code) |
