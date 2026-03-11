# WokSpec — Security Policy

This document covers the security model, responsible disclosure process, and security standards that apply across all WokSpec projects.

---

## Responsible Disclosure

**Do not report security vulnerabilities through public GitHub issues, Discord, or social media.**

Email: security@wokspec.org

Include in your report:
- Affected project(s) and version/commit
- Description of the vulnerability
- Steps to reproduce
- Potential impact

We will acknowledge your report within 48 hours and provide a remediation timeline within 7 days.

---

## Scope

In scope:
- All `*.wokspec.org` domains and subdomains
- All WokSpec GitHub repositories
- The Chopsticks hosted Discord bot instance
- The Eral API and widget

Out of scope:
- Social engineering of WokSpec staff
- Denial-of-service attacks
- Issues in third-party dependencies (report to the upstream project)
- Vulnerabilities in user self-hosted Chopsticks instances (unless the flaw is in the codebase itself)

---

## Auth Security Standards

All WokSpec services must follow these standards:

| Standard | Requirement |
|---|---|
| Passwords | bcrypt with work factor ≥ 12 |
| JWT signing | HS256 with `JWT_SECRET` ≥ 32 bytes entropy |
| JWT expiry | Access tokens: 7 days max; refresh tokens: 30 days |
| Cookies | `httpOnly`, `secure`, `sameSite=strict` for session cookies |
| CSRF | SameSite=strict sufficient for cookie-based auth; additional CSRF token for form submissions |
| OAuth state | Random state parameter validated on callback; stored in httpOnly cookie |

---

## Secret Management Standards

| Rule | Detail |
|---|---|
| Never commit secrets | `.env`, `.env.local`, credential files always in `.gitignore` |
| Use `.env.example` | Committed template file with placeholder values only |
| Rotate on exposure | Any accidentally committed secret must be rotated before the next day |
| Principle of least privilege | Service accounts and API keys scoped to minimum required permissions |
| Edge secrets | Cloudflare Workers secrets via `wrangler secret put`, never in `wrangler.toml` values |

---

## Project-Specific Security Notes

### Chopsticks — Agent Tokens
The highest-risk surface across all WokSpec products. See [Chopsticks Security Doc](./chopsticks/security.md) for the full model.

Key points:
- AES-256-GCM encryption with HKDF-derived per-pool keys
- Revocation via `token = NULL` (never trusted via status flag alone)
- Tokens never written to logs — only masked form displayed

### WokAPI — JWT Trust Model
WokAPI and Eral share `JWT_SECRET`. This means:
- A compromised `JWT_SECRET` allows forging tokens accepted by both services
- The secret must be rotated simultaneously in both Workers if exposed
- Use `wrangler secret put JWT_SECRET` — never commit to `wrangler.toml`

### WokForge — LLM Prompt Injection
The orchestrator constructs prompts from user input. Risks:
- Prompt injection could cause the LLM to output a plan with unintended navigation targets
- Mitigation: validate all generated plan step URLs against an allowlist (configurable) before execution
- Browser automation steps run in isolated sessions (no persistent cookies shared between plans)

### Eral — Widget Key Exposure
`data-eral-key` attributes on the widget script tag are visible in page source. This is intentional — they are intended to be public-facing keys with usage limits, not secret keys. Rate limiting is enforced server-side by `userId` or IP. Treat `eral_*` keys like Stripe publishable keys, not secret keys.

---

## Dependency Security

- Dependabot is enabled across all repositories
- Critical/high severity alerts must be addressed within 14 days
- `npm audit` must produce 0 critical findings in CI

---

## Bug Bounty

WokSpec does not currently operate a paid bug bounty program. We recognise responsible reporters in our changelog and, for significant findings, with WokSpec Pro credits.
