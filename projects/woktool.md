# WokTool

**80+ free browser-based tools for developers and designers. No login. No data leaves your browser.**

- **URL:** [tools.wokspec.org](https://tools.wokspec.org)
- **Repo:** [WokSpec/WokTool](https://github.com/WokSpec/WokTool)
- **Stack:** Next.js · TypeScript · Vercel
- **License:** MIT

---

## What It Is

WokTool is a self-contained utility suite. Every tool runs entirely in the user's browser — no server processing, no accounts, no tracking. You open a page, use the tool, and leave. The goal is to make the most commonly needed one-off tasks (format this JSON, resize this image, generate a UUID, etc.) as frictionless as possible.

Adding a new tool is intentionally simple: one file plus one registry entry.

---

## Tool Categories

### Image
Background Remover, Image Converter (batch), Image Compressor, Image Resizer, Social Media Resizer, Sprite Sheet Packer, Pixel Art Editor, Favicon Generator, Color Palette Extractor, Image Diff

### Design
CSS Generator Suite (gradient, glassmorphism, box-shadow, border-radius), Color Utilities (Hex/RGB/HSL/OKLCH, WCAG contrast, harmonies), Open Graph Preview, Mockup Generator, Font Pairer, Gradient Generator, Icon Search (200k+ icons via Iconify), Website Palette Extractor

### Dev Tools
JSON Toolkit (format / validate / minify / diff / convert), Regex Tester, Encode/Decode (Base64, URL, HTML, Unicode, JWT, Morse, ROT13), Hash Generator (MD5/SHA-1/SHA-256/SHA-512/HMAC), UUID Generator, JWT Debugger, Cron Builder, SQL Formatter, CSV Tools, Diff Tool, Markdown Editor, JSON → TypeScript, Password Generator, Timestamp Converter, HTTP Status Reference

### Text & PDF
Text Utilities (case, lorem ipsum, string ops), Word Counter, PDF Tools (merge/split/compress)

### Game Dev
Tilemap Editor, Sprite Sheet Packer, SFX Browser, Asset Manifest Generator

### Audio
Audio Tools (trim/convert/visualise), Transcribe (speech-to-text with confidence scores via AssemblyAI)

---

## Architecture

Almost entirely client-side. The Next.js App Router is used for routing and layout only; the tool logic runs in-browser. Optional server-tied features (Transcribe, Icon Search via Pixabay) use a thin API route with the key stored server-side.

```
Next.js App Router (Vercel)
  ├── /[tool-slug]     Each tool is its own page/component
  ├── /api/...         Thin proxies for key-gated APIs (optional)
  └── registry.ts      Central tool manifest (name, slug, category, component)
```

---

## Local Development

```bash
git clone https://github.com/WokSpec/WokTool.git
cd WokTool/apps/web
npm install
npm run dev     # http://localhost:3001
```

Optional env vars (all tools work without them):

| Variable | Used by |
|---|---|
| `PIXABAY_API_KEY` | Icon Search |
| `ASSEMBLYAI_API_KEY` | Transcribe |
| `EXA_API_KEY` | Exa Search |

---

## Contributing a New Tool

1. Create `src/app/tools/[slug]/page.tsx` — self-contained React component
2. Add one entry to `src/lib/registry.ts` with `name`, `slug`, `category`, `description`
3. That's it — the tool appears in the sidebar, search, and category pages automatically

See [CONTRIBUTING.md](../../WokTool/CONTRIBUTING.md) for style guidance.

---

## How It Connects to Other Projects

| Project | Relationship |
|---|---|
| **WokGen** | WokTool's Pixel Art Editor and Sprite Sheet Packer share UX heritage with WokGen; WokGen is the AI-powered superset |
| **Vecto** | Vecto's design studios go deeper; WokTool provides quick one-off utilities |
| **WokSite** | Linked from the platform hub as a free flagship tool |
| **Eral** | Planned: Eral available in WokTool sidebar for in-tool AI assistance |

---

## License

[MIT](https://opensource.org/licenses/MIT)
