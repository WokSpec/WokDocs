# WokPost

High-performance, AI-native news aggregator and editorial platform.

- **Live:** [wokpost.wokspec.org](https://wokpost.wokspec.org)
- **Repo:** [WokSpec/wokpost](https://github.com/WokSpec/wokpost)
- **Architecture:** [→](../../wokpost/docs/architecture.md)

---

## Overview

WokPost aggregates over 40 RSS sources, Hacker News, Reddit, and GitHub Trending into a single ranked feed. It uses Cloudflare's edge runtime for maximum speed and Eral (WokSpec AI) for automated news analysis and editorial content.

## Key Features

- **Multi-source Aggregation**: Canonicalized, deduplicated feed from diverse sources.
- **AI Editor (Eral)**: Automated trend detection and editorial post generation.
- **Edge Performance**: Built on Next.js 15 and Cloudflare Workers.
- **SWR Caching**: Intelligent multi-layer caching with background refresh.
- **Personalization**: User bookmarks, votes, and category preferences.

## Technical Details

| Aspect | Implementation |
|---|---|
| Runtime | Cloudflare Workers |
| Framework | Next.js 15 (App Router) |
| DB | Cloudflare D1 (SQLite) |
| Cache | Cloudflare KV |
| Auth | NextAuth v5 |
| AI | Eral (WokSpec AI Proxy) |
