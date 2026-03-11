# WokSpec — Contributing Guide

This guide applies across all WokSpec repositories. Each project may have additional context in its own `CONTRIBUTING.md` — always read that too.

---

## Before You Start

1. **Check existing issues.** Your idea or bug may already be tracked.
2. **For large changes, open a discussion first.** Describe what you want to build and why. This avoids wasted effort.
3. **One concern per PR.** A PR that mixes a bug fix with a refactor with a new feature will not be merged.

---

## Development Workflow

### 1. Fork and Clone
```bash
git clone git@github.com:WokSpec/<repo>.git
cd <repo>
```

Or, if you don't have write access:
```bash
# Fork on GitHub, then:
git clone git@github.com:<your-username>/<repo>.git
git remote add upstream git@github.com:WokSpec/<repo>.git
```

### 2. Create a Branch
Branch names follow `<type>/<description>`:

| Prefix | When to use |
|---|---|
| `feat/` | New features |
| `fix/` | Bug fixes |
| `docs/` | Documentation only |
| `chore/` | Tooling, dependencies, config |
| `refactor/` | Internal restructure (no behaviour change) |
| `test/` | Adding or fixing tests |

Examples: `feat/agent-pool-tiers`, `fix/economy-balance-overflow`, `docs/contributing-guide`

### 3. Run Locally
Each project has its own start command. The most common:
```bash
npm install
npm run dev
```

See the project's `README.md` for prerequisites (env vars, Docker services, etc.).

### 4. Make Your Changes
- **TypeScript everywhere.** No `any`, no `@ts-ignore`, no untyped third-party imports.
- **Follow existing patterns.** Look at how similar features are implemented before inventing something new.
- **Keep functions small.** If a function is over 50 lines, consider splitting it.
- **Write tests for logic.** Don't test implementation details — test behaviour.

### 5. Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org):

```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```

| Type | When |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `chore` | Build tools, dependencies |
| `refactor` | Refactor (no behaviour change) |
| `test` | Tests |
| `perf` | Performance improvement |

Examples:
```
feat(economy): add crafting recipe for rare loot
fix(agents): prevent null token from passing runner check
docs(contributing): add branch naming conventions
```

Breaking changes: append `!` to the type and add a `BREAKING CHANGE:` footer:
```
feat(api)!: change session token format

BREAKING CHANGE: Existing sessions are invalidated. Users must log in again.
```

### 6. Verify Before Opening a PR

Run these before pushing:
```bash
npm run lint     # ESLint
npx tsc --noEmit # TypeScript type check
npm test         # Unit tests
```

All three must pass. PRs with lint or type errors will be returned without review.

### 7. Open a Pull Request
- Title: same format as your commit message
- Body: explain **what** changed and **why**, not just **how**
- Link the related issue: `Closes #123`
- Assign yourself; don't assign reviewers (maintainers self-assign reviews)
- Mark as Draft if it's not ready for review

---

## Code Review

PRs are typically reviewed within 2–3 business days. A maintainer will:
- Leave inline comments for specific lines
- Request changes or approve
- Merge (squash merge into `main`)

**Respond to all comments** before requesting a re-review. Mark your own comments as resolved only after addressing them.

---

## Secrets and Environment Variables

- **Never commit secrets.** `.env.local`, `.env`, and any file containing API keys must be in `.gitignore`.
- Use `.env.example` (committed, no real values) as the template.
- If you accidentally commit a secret: notify security@wokspec.org immediately, rotate the key, and request a git history rewrite.

---

## Project-Specific Notes

| Project | Key things to know |
|---|---|
| **WokSite** | Cloudflare Pages deploy — test with `npm run build:cloudflare` locally |
| **Chopsticks** | Discord bot — test in a private guild; never test with the production bot token |
| **WokAPI** | D1 migrations are permanent — write reversible SQL and test on local D1 first |
| **Eral** | Workers AI models are free-tier — do not switch to OpenAI default in PRs |
| **WokGen / Vecto** | Generation calls cost money — add `NODE_ENV=test` guards around AI provider calls in tests |
| **WokForge** | Browser automation — mock the `BrowserAgent` in unit tests; only integration tests hit real browsers |
| **WokTool** | Tools are client-side only — no server calls in new tool implementations unless a thin proxy is truly required |

---

## License

By contributing, you agree your work is licensed under the repository's stated license. For FSL-1.1-MIT repositories, this means your contributions will be dual-licensed (FSL for 2 years, then MIT).
