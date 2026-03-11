# Vecto

**AI design studio. Vectors, brand kits, UI/UX mockups, voice assets, and code copy — one workspace.**

- **URL:** [vecto.wokspec.org](https://vecto.wokspec.org)
- **Repo:** [WokSpec/Vecto](https://github.com/WokSpec/Vecto)
- **Stack:** Next.js · TypeScript · Prisma (Neon PostgreSQL) · BullMQ + Redis (Upstash) · Stripe · Vercel
- **License:** FSL-1.1-MIT

---

## What It Is

Vecto is WokSpec's design production environment. Where WokGen focuses on pixel art and game assets, Vecto handles the professional creative stack: vector graphics, brand identity, UI wireframes, voice/audio, and written copy. All studios share a unified workspace with Eral providing persistent AI context across sessions.

---

## Studios

| Studio | Output | AI Providers |
|---|---|---|
| **Vector** | SVG icons, illustrations, logos | Recraft, Ideogram |
| **Brand** | Logos, brand kits, social visuals | Replicate + brand context |
| **UI/UX** | Mockups, wireframes, React component sketches | Replicate |
| **Voice** | Speech, narration, audio assets | ElevenLabs |
| **Code** | Copy, docs, SQL, boilerplate code | OpenAI, Groq |

---

## Architecture

Matches WokGen's pattern: Next.js on Vercel, Prisma ORM against Neon (serverless PostgreSQL), async job queuing via BullMQ + Redis (Upstash), Stripe for subscriptions.

```
Next.js App Router (Vercel)
  ├── /vector, /business, /uiux, /voice, /studio   Studio routes
  ├── /eral                                         Eral AI companion
  └── /api
       ├── generate    Enqueues job → BullMQ → provider worker
       ├── billing     Stripe checkout / portal
       └── ...

Background workers (BullMQ)
  ├── RecraftWorker
  ├── IdeogramWorker
  ├── ReplicateWorker
  └── ElevenLabsWorker
```

---

## Local Development

```bash
git clone https://github.com/WokSpec/Vecto
cd Vecto
cp apps/web/.env.example apps/web/.env.local
# Fill in: DATABASE_URL, NEXTAUTH_SECRET, RECRAFT_API_KEY,
#          REPLICATE_API_TOKEN, ELEVENLABS_API_KEY, STRIPE_*, UPSTASH_REDIS_*
npm install --legacy-peer-deps
npm run web:dev     # http://localhost:3000
```

---

## Project Structure

```
Vecto/
├── apps/
│   └── web/
│       ├── src/app/
│       │   ├── vector/    Vector Studio
│       │   ├── business/  Brand Studio
│       │   ├── uiux/      UI/UX Studio
│       │   ├── voice/     Voice Studio
│       │   ├── studio/    Unified studio shell
│       │   ├── eral/      Eral AI companion
│       │   └── api/       API routes
│       └── prisma/        Database schema + migrations
└── packages/              Shared types and utilities
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **Eral** | Companion embedded site-wide; persistent memory + brand context scoped to Vecto projects |
| **WokGen** | Complementary: Vecto for vector/brand/voice, WokGen for pixel/game assets |
| **WokAPI** | Subscription verification (Pro tiers) routed through WokAPI |
| **WokSite** | Listed on the WokSpec platform hub as a core product |
| **WokTool** | WokTool's quick design utilities (CSS generators, color tools) complement Vecto's deep studio |

---

## License

[FSL-1.1-MIT](https://fsl.software) — converts to MIT two years after publication.
