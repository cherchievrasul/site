# Stack Research

**Domain:** Personal static website on shared hosting (Beget)
**Researched:** 2026-06-24
**Confidence:** HIGH

## Recommended Stack

### Core

| Technology | Version | Purpose | Confidence |
|-----------|---------|---------|------------|
| Vite | 8.1.0 | Build tool — scaffolds project, minifies output, dev server with HMR | HIGH |
| Tailwind CSS | 4.3.1 | Utility-first CSS framework — zero config with `@import "tailwindcss"` in v4 | HIGH |
| `@tailwindcss/vite` | 4.3.1 | Vite plugin — auto-processes Tailwind in dev and prod builds | HIGH |
| Lucide | 1.21.0 | Icon library — tree-shakeable, 1500+ icons, ISC license, 0 dependencies | HIGH |
| Google Fonts CDN | — | Typography (Inter) — one `<link>` tag, no npm package needed | HIGH |

### Build output

```
npm run build → dist/
                   index.html
                   assets/
                       style-abc123.css (~10-15 KB gzipped)
                   favicon.ico
```

Pure static files. Zero server-side dependencies. Deploy `dist/` to `public_html/` via FTP.

## What NOT to Use (and Why)

| Technology | Why not |
|-----------|---------|
| React / Vue / Svelte | 40-150 KB framework JS for zero benefit — landing page has no state, no routes |
| Bootstrap | 150+ KB CSS for a grid system — Tailwind does the same in 10 KB via tree-shaking |
| jQuery | 87 KB for DOM APIs that browsers have natively since 2015 |
| Vite `react`/`vue` template | Adds framework boilerplate — `vanilla` template is the right choice |
| Webpack / Parcel | Vite is faster, simpler, and the 2025 standard for static sites |
| PHP | Not needed — site has no dynamic content, no forms (Phase 1) |
| NPM font packages | Google Fonts CDN is simpler, faster, and leverages cross-site caching |

## Deployment

```bash
npm run build
# FTP upload dist/* → public_html/ on Beget
# Files: index.html, assets/*, favicon.ico, .htaccess
```

No build step on the server. Build locally, push `dist/` to FTP.
