# SKELETON.md — Walking Skeleton

**Phase:** 01 — Заглушка + Деплой
**Created:** 2026-06-24

## Architectural Skeleton

The Walking Skeleton proves the thinnest possible end-to-end slice: a single static HTML page built with Vite, styled with Tailwind CSS, and deployed via FTP to Beget — all working in production.

### Stack (locked for entire project)

| Concern | Choice | Version |
|---------|--------|---------|
| Build tool | Vite (vanilla template) | 8.1.0 |
| CSS framework | Tailwind CSS | 4.3.1 |
| CSS plugin | @tailwindcss/vite | 4.3.1 |
| Icons | Lucide | 1.21.0 |
| Typography | Inter (Google Fonts CDN) | — |
| Deployment | FTP → Beget public_html/ | — |

### Design System Foundation

CSS custom properties established from day one in `style.css`:

```css
:root {
  --color-bg: #ffffff;
  --color-text: #111827;
  --color-text-secondary: #4b5563;
  --color-accent: #2563eb;
  --color-accent-hover: #1d4ed8;
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
}
```

This enables dark mode in Phase 3 with a simple `[data-theme="dark"]` block — no painful retrofitting.

### Project Structure

```
project/
  index.html          ← entry point (Vite requires at root)
  vite.config.js      ← Vite + Tailwind plugin
  package.json        ← dependencies + scripts
  src/
    style.css         ← Tailwind imports + custom properties
    main.js           ← empty (no JS yet, placeholder for Phase 2+)
  public/
    favicon.ico       ← favicon
    .htaccess         ← Apache config (copied verbatim to dist/)
  scripts/
    deploy.mjs        ← FTP deployment script
  dist/               ← build output (gitignored)
```

### Data Flow

```
src/style.css → Vite (Tailwind plugin) → dist/assets/style-[hash].css
index.html → Vite (HTML plugin) → dist/index.html
public/* → Vite (copied verbatim) → dist/*
dist/* → FTP → public_html/ on Beget → served via Apache
```

### Decisions That Won't Change

| Decision | Rationale |
|----------|-----------|
| No JS framework (React/Vue/Svelte) | Static site — no state, no routes |
| CSS custom properties in :root | Dark mode foundation, zero cost now |
| .htaccess in public/ | Copied verbatim by Vite, deployed with build |
| Google Fonts CDN over npm | Simpler, leverages cross-site caching |
| dist/ → public_html/ via FTP | Beget's only deployment method for static sites |
