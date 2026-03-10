### NOTE: MAKE SURE TO USE 6.1 INCH simulator to capture starting screenshots
this will save u from adjusting the images later

# App Store & Google Play Screenshots (Safe)

A security-hardened skill for AI-powered coding agents (Claude Code, Cursor, Windsurf, etc.) that generates production-ready screenshots for **iOS App Store** and **Google Play**. It scaffolds a Next.js project, designs advertisement-style screenshots, and exports them at all required resolutions for both stores.

![Example output — PhotoBoost (example2.png)

## Features

- **Dual-store support** — iOS App Store (4 sizes) + Google Play (phone, 7", 10") in one project
- **Brand presets** — define your brand once in `.app-screenshots/brand.yml`, skip repeat setup
- **Auto-detection** — finds app name, icon, and screenshots from Xcode/Android projects
- **Self-verification** — agent checks its own output (legibility, layout variety, exports) before showing you
- **Category-aware copy** — microcopy examples for fitness, productivity, finance, social, and utilities
- **Thumbnail legibility test** — verifies headlines are readable at browse size, not just full resolution
- **Organized exports** — `exports/ios/` and `exports/google-play/` folders, ready for upload

## Security Built In

| Protection | Details |
|------------|---------|
| **Pinned dependencies** | `next@15.1.0`, `html-to-image@1.11.13` — no `@latest` |
| **Input validation** | Color, font name, file path, and text content validated with regex + blocklists |
| **Path traversal prevention** | Rejects `..`, absolute paths, null bytes; image extensions only |
| **Injection prevention** | Blocks `url()`, `expression()`, `eval()`, `javascript:` in user values |
| **Font name allowlist** | Alphanumeric + space + hyphen only, max 60 chars |
| **No dangerous APIs** | `dangerouslySetInnerHTML`, `eval()`, `new Function()`, `fetch()` forbidden |
| **Scoped instructions** | "Additional instructions" limited to visual/design — cannot install packages, run commands, or access files outside project |
| **Filename sanitization** | Non-alphanumeric chars stripped from export filenames |

## Brand Preset (skip repeat setup)

Create `.app-screenshots/brand.yml` in your project root:

```yaml
brand:
  colors:
    accent: "#4A90D9"
    text: "#1A1A2E"
    background: "linear-gradient(135deg, #667eea, #764ba2)"
  font: "Inter"
  style: "clean/minimal"
  icon: "public/app-icon.png"
stores:
  - ios
  - google-play
```

When this file exists, the skill skips brand-related questions and only asks about screenshots, features, and slide count.

## Install

### Using npx skills (project-scoped — recommended)

```bash
npx skills add ofirkris/app-store-screenshots-safe
```

This works with Claude Code, Cursor, Windsurf, OpenCode, Codex, and [40+ other agents](https://github.com/vercel-labs/skills#available-agents).

Install for a specific agent:

```bash
npx skills add ofirkris/app-store-screenshots-safe -a claude-code
```

### Global install (use with caution)

Global installation modifies your AI agent's behavior across **all projects and future sessions**. Prefer project-scoped installation.

```bash
npx skills add ofirkris/app-store-screenshots-safe -g
```

### Manual (git clone — project-scoped)

```bash
# Project-scoped (recommended)
git clone https://github.com/ofirkris/app-store-screenshots-safe .claude/skills/app-store-screenshots

# Global (affects all projects — use with caution)
git clone https://github.com/ofirkris/app-store-screenshots-safe ~/.claude/skills/app-store-screenshots
```

## Usage

Once installed, the skill triggers automatically when you ask your agent to:

- Build App Store screenshots
- Generate Google Play screenshots
- Create marketing screenshots for my iOS/Android app
- Build screenshots for both stores

Or just tell your agent what you need:

```
> Build App Store and Google Play screenshots for my app
```

It will auto-detect what it can from your project, then ask about the rest.

## What gets scaffolded

```
project/
├── .app-screenshots/
│   └── brand.yml           # Brand preset (optional, you create this)
├── public/
│   ├── mockup.png          # iPhone frame (copied from skill)
│   ├── app-icon.png        # Your app icon
│   └── screenshots/        # Your app screenshots
├── src/app/
│   ├── layout.tsx          # Font setup
│   └── page.tsx            # Screenshot generator (single file)
├── exports/                # Organized export output
│   ├── ios/
│   └── google-play/
├── package.json
└── ...
```

The entire generator is a **single `page.tsx` file**. Run the dev server, open the browser, click any screenshot to export it as a PNG.

## Export sizes

### iOS App Store

| Display | Resolution |
|---------|-----------|
| 6.9" | 1320 x 2868 |
| 6.5" | 1284 x 2778 |
| 6.3" | 1206 x 2622 |
| 6.1" | 1125 x 2436 |

### Google Play

| Device | Resolution |
|--------|-----------|
| Phone | 1080 x 1920 |
| 7" Tablet | 1080 x 1920 |
| 10" Tablet | 1920 x 1200 (landscape) |

## Tech stack

| Dependency | Version | Purpose |
|-----------|---------|---------|
| Next.js | 15.1.0 | Dev server + static image serving |
| TypeScript | (bundled) | Type safety |
| Tailwind CSS | (bundled) | Styling |
| html-to-image | 1.11.13 | PNG export at exact resolutions |
| React | (bundled) | Component composition |

## Key design principles

- **Screenshots are ads, not docs** — each slide sells one idea
- **Copy follows the "one second" rule** — readable at thumbnail size in both stores
- **Google Play needs tighter copy** — 2-4 words per line (smaller thumbnails than App Store)
- **Layouts vary** — no two adjacent slides share the same phone placement
- **Style is user-driven** — no hardcoded colors, gradients, or fonts
- **Agent self-verifies** — checks output before showing you, reducing iteration rounds

## Requirements

- Node.js 18+
- One of: bun, pnpm, yarn, or npm (detected automatically, bun preferred)

## License

MIT
