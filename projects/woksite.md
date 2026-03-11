# WokSite

**The platform hub and marketing presence for WokSpec.**

- **URL:** [wokspec.org](https://wokspec.org)
- **Repo:** [WokSpec/WokSite](https://github.com/WokSpec/WokSite)
- **Stack:** Next.js · TypeScript · Cloudflare Pages (OpenNext)

---

## What It Is

WokSite is the main website and product shell for WokSpec — the public-facing entry point for everything the organisation builds. It hosts the marketing pages for every product (Eral, Dilu, Vecto, WokPost, WokTool, Chopsticks), the authentication layer used across the platform, a user dashboard, a contact form backed by Resend, and a shared component library.

It is not a back-end in the traditional sense. All heavy logic lives in WokAPI or each product's own service. WokSite ties them together at the UI level: you log in here, navigate to products from here, and return here for docs, status, and account management.

---

## Key Pages

| Route | Purpose |
|---|---|
| `/` | Homepage — product overview and CTAs |
| `/dashboard` | Auth-gated user dashboard |
| `/docs` | Product documentation |
| `/projects` | Showcase of WokSpec projects |
| `/wokgen` | WokGen product landing |
| `/about`, `/services` | Company info |
| `/status` | Live platform status |
| `/privacy`, `/terms` | Legal pages |
| `api/contact` | Contact form handler (Resend) |
| `api/auth/github` | GitHub OAuth flow |

---

## Architecture

WokSite is a **Next.js App Router** application compiled for **Cloudflare Pages** using `@opennextjs/cloudflare`. Every page is server-rendered at the edge; no separate origin server is required.

Auth is handled with a custom JWT flow (GitHub OAuth → JWT issued by the app → stored in a cookie). The `JWT_SECRET` is shared with WokAPI so tokens are valid platform-wide. A Cloudflare Workers KV namespace (`CONTACTS_KV`) backs the contact form.

```
WokSite (Cloudflare Pages)
  ├── Auth (GitHub OAuth → JWT)
  ├── Dashboard (reads WokAPI)
  ├── Contact form (Cloudflare KV + Resend)
  └── Static marketing pages
```

---

## Local Development

```bash
git clone git@github.com:WokSpec/WokSite.git
cd WokSite
npm install
cp .env.example .env.local   # fill in values
npm run dev                  # http://localhost:3000
```

**Required env vars:** `RESEND_API_KEY`, `RESEND_AUDIENCE_ID`, `CONTACT_EMAIL`, `JWT_SECRET`, `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`

---

## Deploy

```bash
npm run deploy   # OpenNext build → wrangler pages deploy
```

Every push to `main` triggers the GitHub Actions pipeline: lint → TypeScript check → build → Cloudflare Pages deploy.

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **WokAPI** | Dashboard reads auth and subscription state from the API |
| **Eral** | Eral widget is embedded on the marketing site |
| **Chopsticks** | Has its own subdomain site (`chopsticks.wokspec.org`) built inside `/Chopsticks/web` |
| **WokGen, Vecto, Dilu** | Linked from product pages; each has its own subdomain |
| **WokTool** | Linked at `tools.wokspec.org` |

---

## License

© WokSpec. Eral and Dilu are source-available under [FSL-1.1-MIT](https://fsl.software). WokTool and Chopsticks are [MIT](https://opensource.org/licenses/MIT).
