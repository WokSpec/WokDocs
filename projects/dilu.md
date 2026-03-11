# Dilu

**No-code product launcher. Pick a template, connect your tools, set a domain, ship.**

- **URL:** [dilu.wokspec.org](https://dilu.wokspec.org)
- **Repo:** [WokSpec/Dilu](https://github.com/WokSpec/Dilu)
- **Stack:** Next.js · TypeScript · Cloudflare Pages (OpenNext)
- **License:** FSL-1.1-MIT

---

## What It Is

Dilu removes the friction between "I have an idea" and "it's live." It provides production-ready templates hooked into real integrations (Stripe, Shopify, Discord, Resend) and handles domain setup, deployment, and configuration without requiring users to touch a config file or run a terminal command.

Templates are not starters — they're complete, deployable products that Dilu manages on the user's behalf.

---

## Templates

| Template | Built for |
|---|---|
| **Landing page** | Marketing sites, waitlists, product announcements |
| **SaaS starter** | Auth, payments, dashboard, user management |
| **Discord community** | Bot setup, role management, onboarding flows |
| **Creator store** | Digital products, subscriptions, membership |
| **Portfolio** | Case studies, work showcase, contact |

---

## Integrations

All OAuth-based — no manual API key setup required.

| Category | Services |
|---|---|
| Payments | Stripe, Lemon Squeezy |
| E-commerce | Shopify |
| Community | Discord |
| Email | Resend, Mailchimp |
| CMS | Notion, Airtable |
| Auth | GitHub, Google |
| Analytics | Plausible, Fathom |

---

## How It Works

1. User picks a template
2. Connects integrations via OAuth (no config files)
3. Sets a domain (free `*.dilu.app` subdomain or custom)
4. Launches — Dilu handles the deploy

---

## Pricing

| | Free | Builder |
|--|---|---|
| Projects | 1 | Unlimited |
| Domain | `*.dilu.app` | Custom domain |
| Integrations | 3 | All |
| Eral AI | Basic | Full |

---

## Architecture

Dilu deploys on the same Cloudflare Pages + OpenNext stack as WokSite. The integration layer uses OAuth token exchange per-user, with tokens stored in Cloudflare KV scoped to the user's account. Template deployments are forked into isolated Cloudflare Pages projects under a WokSpec account.

```
Dilu (Cloudflare Pages)
  ├── Template catalog
  ├── Integration OAuth flows (per-user token storage in KV)
  ├── Domain management (Cloudflare proxy + DNS)
  └── Deployment engine (forks templates → CF Pages projects)
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **WokAPI** | Subscription and account management of Dilu tiers run through WokAPI |
| **Eral** | Eral AI companion available in Builder tier; embedded via widget |
| **WokSite** | Dilu is a WokSpec product — listed on the platform hub and accessible via the user dashboard |
| **Chopsticks** | "Discord community" template pre-configures Chopsticks as the server bot |

---

## License

[FSL-1.1-MIT](https://fsl.software) — free for personal and non-commercial use. Converts to MIT two years after publication.
