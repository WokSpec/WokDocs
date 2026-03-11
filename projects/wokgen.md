# WokGen

**AI pixel art studio — generate sprites, tilesets, animations, and game-ready assets in the browser.**

- **URL:** [wokgen.wokspec.org](https://wokgen.wokspec.org)
- **Repo:** [WokSpec/WokGen](https://github.com/WokSpec/WokGen)
- **Stack:** Next.js · TypeScript · Prisma (Neon PostgreSQL) · BullMQ + Redis (Upstash) · Vercel

---

## What It Is

WokGen is a browser-based pixel art production suite powered by multiple AI backends. It covers the full game-asset workflow: prompt → generate → animate → edit → export. The pixel editor works entirely offline; no API keys are needed to draw.

Eral is integrated site-wide, providing a persistent AI companion that understands your active project, remembers previous sessions, and can trigger generation jobs directly from chat.

---

## Tools

| Tool | What it does |
|---|---|
| **AI Generator** | Text-to-pixel-art with 18 style presets and multiple aspect ratios |
| **Animator** | Multi-frame sprite animation editor; exports looping GIFs |
| **Scene Builder** | Generates coherent tilesets and environment scenes from a single prompt |
| **Pixel Editor** | Browser-based canvas editor (pencil, fill, eraser, palette, PNG export) — works offline |
| **Gallery** | Browse and save community generations |

---

## AI Providers

| Provider | Role |
|---|---|
| Fal.ai / FLUX | Default generation — fast, cost-efficient |
| FLUX Pro via Replicate | HD quality generation |
| Real-ESRGAN | Upscaling low-res output |
| ControlNet | Sketch-guided and palette-guided generation |

---

## Architecture

```
Next.js App Router (Vercel)
  ├── /pixel          AI generation routes
  ├── /editor         Offline canvas editor (no server needed)
  ├── /gallery        Community gallery (Prisma + Neon)
  ├── /eral           Eral AI companion (proxied to Eral API)
  └── /api
       ├── generate   Enqueues job → BullMQ → Fal/Replicate worker
       └── ...
```

Generation jobs are queued through BullMQ with Redis (Upstash) so the Next.js serverless functions return immediately and clients poll for results.

---

## Local Development

```bash
git clone https://github.com/WokSpec/WokGen
cd WokGen
cp apps/web/.env.example apps/web/.env.local
# Fill in: DATABASE_URL, NEXTAUTH_SECRET, FAL_API_KEY, REPLICATE_API_TOKEN, UPSTASH_REDIS_*
npm install --legacy-peer-deps
npm run web:dev     # http://localhost:3000
```

The `/editor` route works with no env vars — great starting point for contributors.

---

## Project Structure

```
WokGen/
├── apps/
│   └── web/
│       ├── src/app/        Next.js pages (pixel, editor, gallery, eral, api)
│       └── prisma/         Database schema
└── packages/               Shared utilities
```

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **Eral** | Eral companion embedded in every WokGen page; can trigger generation from chat |
| **WokTool** | WokTool's Sprite Sheet Packer and Pixel Art Editor share heritage; WokGen is the AI-powered evolution |
| **Vecto** | Vecto handles vectors, brand, UI/UX; WokGen handles pixel/game assets — complementary studios |
| **WokSite** | WokGen has a landing page at `/wokgen` on WokSite; linked from the platform hub |

---

## License

[FSL-1.1-MIT](https://fsl.software) (source-available) — converts to MIT two years after publication.
