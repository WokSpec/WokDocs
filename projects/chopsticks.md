# Chopsticks

**Open-source Discord bot — music, moderation, economy, AI, and a community agent pool system.**

- **URL:** [chopsticks.wokspec.org](https://chopsticks.wokspec.org)
- **Repo:** [WokSpec/Chopsticks](https://github.com/WokSpec/Chopsticks)
- **Stack:** Node.js 22 · discord.js v14 · PostgreSQL · Redis · Lavalink · Docker
- **License:** MIT + Commercial Attribution

---

## What It Is

Chopsticks is a full-featured, production-grade Discord bot. It ships 70+ slash commands across music, moderation, economy, games, AI, and server management — all configurable per-server through an interactive `/setup` wizard.

The flagship feature is the **Agent Pool System**: community members contribute their own Discord bot tokens to shared pools. Other servers pull from these pools on demand, getting extra bots for voice, assistant, or utility workloads without managing tokens themselves. Tokens are encrypted at rest with AES-256-GCM and never exposed in logs or embeds.

---

## Feature Areas

| Area | Highlights |
|---|---|
| **Music** | Lavalink-backed playback from YouTube, Spotify, SoundCloud, direct URL; full queue management |
| **Moderation** | Ban, kick, mute, warn, timeout, purge (with filters); mod log integration; hierarchy safety |
| **Economy** | Credits, daily/work/gather loops, shop, crafting, auctions, heist, casino, XP + leveling |
| **AI** | `/ai chat` (LLM), `/ai image` (FLUX.1-schnell via HuggingFace), agent-in-voice |
| **Games** | Trivia (solo/PvP/fleet/agent-duel), battle, casino, truth-or-dare, roast, and more |
| **Server Management** | Reaction roles, welcome, giveaways, polls, ticket system, scheduling, starboard, automation |
| **Agent Pool** | Deploy community bots to voice channels; spend credits on agent actions |
| **Dashboard** | Web-based OAuth2 guild config panel; real-time dev dashboard via Socket.io |
| **Observability** | Prometheus metrics, Grafana dashboards, Pino structured logging |

---

## Architecture

```
Discord Gateway
    │
discord.js v14 bot (src/index.js)
    ├── Commands  (src/commands/)
    ├── AgentManager  WebSocket control plane (src/agents/agentManager.js)
    ├── AgentRunner   spawns agent Discord clients (src/agents/agentRunner.js)
    ├── Dashboard     Express + OAuth2 (src/dashboard/)
    └── Storage       PostgreSQL via pg (src/utils/storage_pg.js)
                      Redis cache via ioredis
```

The **AgentManager** is a WebSocket server that routes work orders to live agent clients. The **AgentRunner** polls the database every 10 seconds, creates `discord.js` Client instances for active tokens, and connects them to the manager. Guilds dispatch work through the manager via session IDs.

---

## Self-hosting (Quick)

```bash
git clone https://github.com/wokspec/Chopsticks.git
cd Chopsticks
npm install
cp .env.example .env    # fill in DISCORD_TOKEN, CLIENT_ID, DATABASE_URL, REDIS_URL
npm run migrate
node scripts/deployCommands.js
npm run bot
```

Or spin up the full stack with Docker:

```bash
docker compose -f docker-compose.laptop.yml up -d
```

See [QUICKSTART.md](../../Chopsticks/QUICKSTART.md) for a complete walkthrough including Lavalink setup.

---

## Website

The `web/` folder inside the repo is a Next.js site deployed to **[chopsticks.wokspec.org](https://chopsticks.wokspec.org)** via Cloudflare Pages. It provides: landing page, feature breakdown, searchable command reference, self-hosting walkthrough, tutorials, and changelog.

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **WokSite** | Linked from the main platform; chopsticks.wokspec.org is its own deployment |
| **Eral** | Chopsticks calls Eral (via HuggingFace or direct) for `/ai chat` and image gen |
| **WokAPI** | Planned: subscription/tier verification routed through WokAPI |
| **WokForge** | Planned: automation recipes executable via Chopsticks agent actions |

---

## License

MIT + Commercial Attribution — free to self-host, fork, and white-label. Commercial use requires visible attribution to "Powered by Chopsticks (WokSpec)".
