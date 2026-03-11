# WokAPI

**Core API for WokSpec. Handles auth, sessions, payments, and AI routing across all products.**

- **URL:** [api.wokspec.org](https://api.wokspec.org)
- **Repo:** [WokSpec/WokAPI](https://github.com/WokSpec/WokAPI)
- **Stack:** Cloudflare Workers · Hono · Cloudflare D1 (SQLite at edge) · Cloudflare KV · Stripe

---

## What It Is

WokAPI is the shared back-end that every WokSpec product authenticates against. It owns two concerns:

1. **Identity** — user registration, login, JWT issuance, session management, GitHub and Google OAuth flows
2. **Commerce** — Stripe subscription management, checkout session creation, billing portal, webhook processing

AI calls are proxied through WokAPI to Eral (`/v1/ai/*`) so products can hit a single stable endpoint without caring about which Eral instance or model is in use. The `JWT_SECRET` is shared between WokAPI and Eral so both can issue and verify the same tokens.

---

## API Reference

### Auth
| Method | Path | Description |
|---|---|---|
| `POST` | `/v1/auth/login` | Email/password → JWT |
| `POST` | `/v1/auth/register` | Create account |
| `POST` | `/v1/auth/refresh` | Refresh JWT (30-day rolling) |
| `POST` | `/v1/auth/logout` | Invalidate session |
| `GET` | `/v1/auth/me` | Current user profile |

### Sessions
| Method | Path | Description |
|---|---|---|
| `GET` | `/v1/sessions` | List active sessions |
| `DELETE` | `/v1/sessions/:id` | Revoke a session |

### AI (proxy → Eral)
| Method | Path | Description |
|---|---|---|
| `POST` | `/v1/ai/chat` | Conversational AI |
| `POST` | `/v1/ai/generate` | Content generation |
| `POST` | `/v1/ai/analyze` | Content analysis |

### Billing
| Method | Path | Description |
|---|---|---|
| `POST` | `/v1/billing/checkout` | Create Stripe checkout session |
| `POST` | `/v1/billing/portal` | Open billing portal |
| `POST` | `/v1/billing/webhook` | Stripe webhook handler |
| `GET` | `/v1/billing/subscription` | Current subscription status |

---

## Auth Model

All protected routes require `Authorization: Bearer <jwt>`. JWTs are HS256 signed with `JWT_SECRET`. Access tokens expire in **7 days**; refresh tokens extend to **30 days** on use. The same secret is shared with Eral so Eral can independently verify tokens without calling back to WokAPI.

---

## Infrastructure

| Layer | Technology |
|---|---|
| Runtime | Cloudflare Workers (globally distributed, zero cold-start) |
| Framework | Hono — lightweight, edge-optimised router |
| Database | Cloudflare D1 (SQLite at the edge) |
| Sessions / Cache | Cloudflare KV |
| Payments | Stripe (checkout, portal, webhooks) |

---

## Local Development

```bash
cd WokAPI
npm install
npm run dev       # wrangler dev on :8787

# Secrets (via wrangler)
wrangler secret put JWT_SECRET
wrangler secret put STRIPE_SECRET_KEY
wrangler secret put STRIPE_WEBHOOK_SECRET
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **Eral** | Shares `JWT_SECRET`; WokAPI proxies AI requests to Eral |
| **WokSite** | Dashboard reads user + subscription state from WokAPI |
| **Dilu** | Subscription tier (Free / Builder) managed through WokAPI |
| **Vecto, WokGen** | Paid features gated via subscription status from WokAPI |
| **WokForge** | Automation usage tiers verified through WokAPI |
| **Chopsticks** | Planned: hosted instance subscription management |
