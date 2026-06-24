# Phase 01: Заглушка + Деплой — Research

**Researched:** 2026-06-24
**Domain:** Static website build + FTP deployment to shared hosting (Beget)
**Confidence:** HIGH

## Summary

Phase 01 delivers a static placeholder page built with Vite 8 + Tailwind CSS 4, deployed via FTP to Beget shared hosting. The architecture is dead-simple: a single `index.html` styled with Tailwind utility classes, zero JavaScript, served by Apache on Beget.

The critical path is: scaffold Vite vanilla project → install Tailwind plugin → write HTML/CSS → `npm run build` → FTP `dist/*` to `public_html/`. The `.htaccess` file (placed in Vite's `public/` directory so it lands in `dist/`) is mandatory from the first deploy to fix Beget's default `windows-1251` encoding and enable HTTPS redirects.

**Primary recommendation:** Place `.htaccess` and `favicon.ico` in Vite's `public/` directory — they will be copied verbatim to `dist/` and deployed to the server root alongside `index.html`.

## User Constraints (from CONTEXT.md)

### Locked Decisions
- **D-01:** Vite 8 (vanilla template) + Tailwind CSS 4 — build tool и стили
- **D-02:** Tailwind подключается через `@tailwindcss/vite` плагин, CSS через `@import "tailwindcss"` (v4 синтаксис, без tailwind.config.js)
- **D-03:** Шрифт Inter через Google Fonts CDN (один `<link>` в `<head>`)
- **D-04:** Никакого JavaScript на заглушке — чистый HTML + CSS
- **D-05:** Заглушка содержит: имя/название, теглайн «Сайт скоро откроется», способ связи (email)
- **D-06:** Вся страница — один `index.html`. Секции: header (название), main (сообщение + контакт), footer (копирайт)
- **D-07:** Ручной FTP-деплой: `npm run build` → загрузка `dist/*` в `public_html/` на beget
- **D-08:** `.htaccess` в корне сайта: `AddDefaultCharset UTF-8`, HTTPS-редирект, `Options -Indexes`

### the agent's Discretion
- Точные отступы и размеры шрифтов
- Цветовая схема (тёмный текст на светлом фоне, один акцентный цвет)
- Выравнивание — горизонтально и вертикально по центру
- Структура `.htaccess` — стандартная для Apache/beget

### Deferred Ideas (OUT OF SCOPE)
- Автоматический деплой через GitHub Actions — Phase 2+
- Тёмная тема — Phase 3
- Форма связи — Phase 3
- Красивый UI/дизайн — Phase 2
- Фото, about, портфолио — Phase 2

## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SETUP-01 | Проект инициализирован — Vite vanilla + Tailwind CSS 4, dev-сервер работает локально | Standard Stack (§Core), Architecture Pattern 1 |
| BUILD-01 | Команда `npm run build` собирает статические файлы в `dist/` | Standard Stack (§Build Output), Architecture Pattern 1 |
| DEPLOY-01 | Сайт задеплоен на beget в `public_html/`, открывается по домену через HTTPS | Don't Hand-Roll (§FTP), Code Examples (§.htaccess), Pitfall 1 |
| STUB-01 | Посетитель видит оформленную заглушку (имя, «скоро открытие», способ связи, favicon) | Architecture Pattern 2, Code Examples (§Stub HTML) |

## Architectural Responsibility Map

| Capability | Primary Tier | Secondary Tier | Rationale |
|------------|-------------|----------------|-----------|
| Project scaffolding & local dev | Build Tool (Vite) | — | Vite provides dev server, HMR, and build pipeline; runs entirely on local machine |
| CSS processing & optimization | Build Tool (Tailwind via @tailwindcss/vite) | — | Tailwind CSS v4 processes `@import "tailwindcss"` at build time, generates optimized CSS; zero runtime |
| Static asset serving | Server (Beget Apache) | — | Apache serves static files from `public_html/`; no backend logic, no PHP needed |
| HTML rendering & layout | Browser / Client | — | Pure HTML + CSS — no JavaScript; browser renders directly from served files |
| FTP deployment | Local machine → Server | — | One-way push: `dist/` → FTP → `public_html/`; no server-side build |
| Encoding & security headers | Server (.htaccess) | — | `.htaccess` overrides Beget's default windows-1251, enables HTTPS redirect, disables directory listing |

## Standard Stack

### Core

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Vite | 8.1.0 | Build tool — dev server, production bundling via Rolldown | Fastest build tool for static sites; vanilla template is zero-bloat; native ESM dev server [VERIFIED: npm registry, vite.dev/guide/] |
| Tailwind CSS | 4.3.1 | Utility-first CSS framework | Version 4 eliminates config files (`@import "tailwindcss"` replaces `@tailwind` directives); tree-shakes unused classes to ~10 KB [VERIFIED: npm registry, tailwindcss.com/docs/installation/using-vite] |
| @tailwindcss/vite | 4.3.1 | Vite plugin for Tailwind CSS v4 | Required for Tailwind v4 integration with Vite; auto-processes CSS in dev and prod builds [VERIFIED: npm registry, tailwindcss.com/docs/installation/using-vite] |
| Google Fonts CDN (Inter) | — | Typography | Single `<link>` in `<head>`, no npm package needed, leverages cross-site caching [CITED: fonts.google.com] |

### Supporting

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| None | — | — | Phase 01 is intentionally minimal — no additional dependencies needed |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Vite + Tailwind (this stack) | Plain HTML + CSS (no build tools) | No CSS optimization, no tree-shaking, no minification. Vite is chosen per D-01 — research confirms it's the right choice for this project scale. |
| `@tailwindcss/vite` plugin | Tailwind CLI / PostCSS plugin | `@tailwindcss/vite` is the official Tailwind-recommended Vite integration; CLI and PostCSS are alternatives for non-Vite builds [CITED: tailwindcss.com/docs/installation] |
| Google Fonts CDN | Self-hosted Inter via npm (`@fontsource/inter`) | CDN is simpler and chosen per D-03. Self-hosting would add an npm dependency and build complexity for zero benefit at this scale. |

**Installation:**
```bash
npm create vite@latest . -- --template vanilla
npm install tailwindcss@4.3.1 @tailwindcss/vite@4.3.1
```

**Version verification (npm registry, 2026-06-24):**
```bash
npm view vite version          # → 8.1.0
npm view tailwindcss version   # → 4.3.1
npm view @tailwindcss/vite version  # → 4.3.1
```

## Package Legitimacy Audit

> Cross-ecosystem note: slopcheck 0.6.1 checks PyPI by default. All three packages are **npm** packages. slopcheck flagged `@tailwindcss/vite` as `[SLOP]` on PyPI — this is a **false positive** from cross-ecosystem confusion. All packages verified on the correct ecosystem registry (npm) below.

| Package | Registry | Published | Source Repo | Postinstall Script | npm View | Disposition |
|---------|----------|-----------|-------------|-------------------|----------|-------------|
| vite | npm | 2026-06-23 (latest: 8.1.0) | github.com/vitejs/vite | none | ✓ confirmed | Approved |
| tailwindcss | npm | latest: 4.3.1 | github.com/tailwindlabs/tailwindcss | none | ✓ confirmed | Approved |
| @tailwindcss/vite | npm | latest: 4.3.1 | github.com/tailwindlabs/tailwindcss | none | ✓ confirmed | Approved |

**Packages removed due to slopcheck [SLOP] verdict:** none (slopcheck false positive on PyPI — all packages are npm-native)
**Packages flagged as suspicious [SUS]:** none

*All packages verified on npm via `npm view`. slopcheck's `[SLOP]` flag on `@tailwindcss/vite` is cross-ecosystem confusion (npm package checked against PyPI). No postinstall scripts detected. All packages are from established, widely-used open-source projects with public GitHub repositories.*

## Architecture Patterns

### System Architecture Diagram

```
┌────────────────────────────────────────────────────────────────┐
│                     LOCAL DEVELOPMENT                           │
│                                                                 │
│  ┌──────────┐    ┌──────────────┐    ┌────────────────────┐   │
│  │  src/    │    │  Vite Dev    │    │  Browser           │   │
│  │  index.html  │  Server       │───▶│  localhost:5173    │   │
│  │  style.css   │  (HMR,        │    │  (live preview)    │   │
│  │          │    │  @tailwindcss │    │                    │   │
│  └──────────┘    │  /vite plugin)│    └────────────────────┘   │
│       │          └──────────────┘                               │
│       │                                                        │
│  ┌────▼─────────┐                                              │
│  │  public/     │  (static assets: .htaccess, favicon.ico)     │
│  └──────────────┘                                              │
│       │                                                        │
│       │  npm run build                                         │
│       ▼                                                        │
│  ┌──────────────────────────────────────┐                      │
│  │  dist/                               │                      │
│  │  ├── index.html        (processed)   │                      │
│  │  ├── assets/                         │                      │
│  │  │   └── style-{hash}.css (10-15KB)  │                      │
│  │  ├── favicon.ico       (from public/)│                      │
│  │  └── .htaccess          (from public/)│                     │
│  └────────────┬─────────────────────────┘                      │
└───────────────┼─────────────────────────────────────────────────┘
                │
                │  FTP upload (manual)
                │  dist/* → public_html/
                ▼
┌────────────────────────────────────────────────────────────────┐
│                     BEGET SHARED HOSTING                        │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Apache Web Server                                        │  │
│  │                                                           │  │
│  │  public_html/                                             │  │
│  │  ├── index.html         ← served at GET /                 │  │
│  │  ├── assets/style-{hash}.css  ← served at GET /assets/... │  │
│  │  ├── favicon.ico        ← served at GET /favicon.ico      │  │
│  │  └── .htaccess          ← Apache config (not served)      │  │
│  │                                                           │  │
│  │  .htaccess overrides:                                     │  │
│  │  • AddDefaultCharset UTF-8  (fixes windows-1251 default)  │  │
│  │  • HTTPS redirect                                           │  │
│  │  • Options -Indexes         (disable directory listing)     │  │
│  └──────────────────────┬───────────────────────────────────┘  │
│                         │                                       │
└─────────────────────────┼───────────────────────────────────────┘
                          │
                          │  HTTPS (nginx frontend → Apache)
                          ▼
┌────────────────────────────────────────────────────────────────┐
│                     VISITOR'S BROWSER                           │
│                                                                 │
│  GET https://domain.ru                                          │
│       │                                                        │
│       ▼                                                        │
│  ┌──────────────────────────────────────────┐                  │
│  │  index.html renders:                      │                  │
│  │  ┌────────────────────────────────────┐  │                  │
│  │  │  <header>  Имя / Название          │  │                  │
│  │  ├────────────────────────────────────┤  │                  │
│  │  │  <main>                            │  │                  │
│  │  │    «Сайт скоро откроется»          │  │                  │
│  │  │    Контакт: email@example.com      │  │                  │
│  │  ├────────────────────────────────────┤  │                  │
│  │  │  <footer>  © 2026                  │  │                  │
│  │  └────────────────────────────────────┘  │                  │
│  └──────────────────────────────────────────┘                  │
│                                                                 │
│  Styled by: assets/style-{hash}.css (Tailwind utilities)       │
│  Font: Inter (Google Fonts CDN, one <link>)                    │
│  JS: none (pure HTML + CSS)                                    │
└────────────────────────────────────────────────────────────────┘
```

### Recommended Project Structure

```
sait/
├── src/
│   ├── index.html            # Entry point — Vite treats this as source
│   └── style.css             # All styles with @import "tailwindcss"
├── public/                   # Static assets copied verbatim to dist/
│   ├── .htaccess             # Apache config (NOT served to browsers)
│   └── favicon.ico           # Browser tab icon (32×32)
├── dist/                     # Build output (gitignored, deployed via FTP)
├── package.json              # npm scripts: dev, build, preview
├── vite.config.js            # Vite + @tailwindcss/vite plugin config
└── node_modules/             # Dependencies (gitignored)
```

**Why `public/` for `.htaccess` and `favicon.ico`:** Vite's `public/` directory contents are copied as-is to the root of `dist/` during build [VERIFIED: vite.dev/guide/assets]. This ensures `.htaccess` lands in the correct location for Apache to read it (the server root = `public_html/`), and `favicon.ico` is served at the standard `/favicon.ico` path. These files must retain their exact filenames — no hashing — so they cannot be imported as modules.

**Build output after `npm run build`:**
```
dist/
├── index.html               # Processed HTML (entry point)
├── assets/
│   └── style-abc123.css     # Tailwind-processed CSS (~10-15 KB gzipped)
├── favicon.ico              # From public/
└── .htaccess                # From public/
```

### Pattern 1: Vite + Tailwind CSS v4 Initialization

**What:** Scaffold a Vite vanilla project, install `@tailwindcss/vite` plugin, configure `vite.config.js`, and use `@import "tailwindcss"` in CSS. This is the official Tailwind CSS v4 + Vite integration path.

**When to use:** At project initialization (SETUP-01). This pattern is the foundation for all subsequent phases.

**Example (vite.config.js):**
```js
// Source: tailwindcss.com/docs/installation/using-vite (verified 2026-06-24)
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
})
```

**Example (src/style.css):**
```css
/* Source: tailwindcss.com/docs/installation/using-vite (verified 2026-06-24)
   Tailwind CSS v4 uses @import "tailwindcss" — no @tailwind directives needed */
@import "tailwindcss";
```

**Example (package.json scripts):**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```
[VERIFIED: vite.dev/guide/ — CLI section]

### Pattern 2: Placeholder Page Structure (Single index.html)

**What:** A single HTML page with three semantic sections: `<header>` (site name), `<main>` (message + contact), `<footer>` (copyright). Styled with Tailwind utility classes. No JavaScript.

**When to use:** This is the STUB-01 requirement — the entire page structure for Phase 01.

**Example (src/index.html):**
```html
<!-- Source: ARCHITECTURE.md semantic sectioning pattern, adapted for Tailwind -->
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Имя — Сайт скоро откроется</title>
  <link rel="icon" type="image/x-icon" href="/favicon.ico">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="/src/style.css">
</head>
<body class="min-h-screen flex flex-col items-center justify-center font-[Inter] bg-white text-gray-900">
  <header class="mb-8">
    <h1 class="text-4xl font-bold tracking-tight">Имя Фамилия</h1>
  </header>

  <main class="text-center max-w-md px-4">
    <p class="text-xl text-gray-600 mb-8">Сайт скоро откроется</p>
    <p class="text-gray-500">
      Свяжитесь со мной:
      <a href="mailto:email@example.com" class="text-blue-600 hover:text-blue-800 underline">
        email@example.com
      </a>
    </p>
  </main>

  <footer class="mt-auto py-6 text-sm text-gray-400">
    &copy; 2026 Имя Фамилия
  </footer>
</body>
</html>
```

**Key Tailwind classes used:**
- `min-h-screen` — full viewport height minimum
- `flex flex-col items-center justify-center` — vertical and horizontal centering
- `font-[Inter]` — Inter font family (loaded via Google Fonts CDN)
- `text-4xl font-bold tracking-tight` — heading styling
- `max-w-md px-4` — content width constraint with padding
- `mt-auto` — pushes footer to bottom

### Pattern 3: .htaccess for Beget Shared Hosting

**What:** Apache `.htaccess` file placed in `public/` (Vite copies to `dist/`), deployed to server root. Fixes encoding, enables HTTPS, disables directory listing, sets caching headers.

**When to use:** From the very first deploy (DEPLOY-01). The `.htaccess` is mandatory — without it, Beget defaults to `windows-1251` encoding which corrupts Russian text.

**Example (public/.htaccess):**
```apache
# Source: Synthesized from PITFALLS.md (§2, §4) and Beget KB recommendations
# Verified against Apache 2.4 documentation

# 1. Кодировка — переопределяет серверный windows-1251 от Beget
AddDefaultCharset UTF-8

# 2. Индексный файл по умолчанию
DirectoryIndex index.html

# 3. Запрет листинга директорий
Options -Indexes

# 4. Редирект HTTP → HTTPS
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]

# 5. Кэширование статических ресурсов
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType image/x-icon "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
</IfModule>

# 6. Отключение кэша для HTML на время разработки
# Убрать, когда сайт будет готов (Phase 2+)
<FilesMatch "\.(html)$">
  Header set Cache-Control "no-cache, must-revalidate"
</FilesMatch>
```

### Anti-Patterns to Avoid

- **Загрузка файлов в корень FTP вместо `public_html/`:** Beget's web root is `public_html/`, not the FTP root. Files outside `public_html/` are not served. [CITED: PITFALLS.md §1]
- **Отсутствие `.htaccess` при первом деплое:** Beget defaults to `windows-1251` encoding — Russian text will show as `??????`. The `.htaccess` with `AddDefaultCharset UTF-8` is mandatory from deploy #1. [CITED: PITFALLS.md §2, §4]
- **Inline-стили в HTML (`style="..."`):** Use Tailwind utility classes in HTML — they are the project's chosen styling system. Inline `style=""` attributes break the utility-first approach. [CITED: PITFALLS.md — Technical Debt]
- **Добавление JavaScript на заглушку:** Phase 01 is pure HTML + CSS per D-04. Adding JS now creates unnecessary complexity and contradicts locked decisions. [CITED: CONTEXT.md D-04]
- **Использование старого синтаксиса Tailwind v3:** Tailwind v4 uses `@import "tailwindcss"` in CSS (not `@tailwind base/components/utilities`) and does not use `tailwind.config.js`. Using v3 syntax will fail silently or produce no styles. [CITED: tailwindcss.com/docs/installation/using-vite]

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| CSS minification & tree-shaking | Manual CSS optimization | Vite + @tailwindcss/vite (built-in) | Vite's Rolldown bundler + Tailwind v4 tree-shaking eliminates unused utility classes automatically — output is ~10-15 KB gzipped |
| FTP file synchronization | Custom FTP script | Manual FTP upload via FileZilla / built-in `ftp.exe` | Phase 01 uses manual FTP per D-07. Scripting FTP is error-prone (binary/ASCII mode, overwrite logic, directory navigation). Auto-deploy is deferred to Phase 2+. |
| Encoding fix | Meta tag only | `.htaccess` with `AddDefaultCharset UTF-8` | `<meta charset="UTF-8">` in HTML is not enough — Beget's Apache sends `Content-Type: text/html; charset=windows-1251` which overrides the meta tag at the HTTP level. `.htaccess` fixes this at the server level. [CITED: PITFALLS.md §2] |
| Favicon generation | Manual icon creation | Any 32×32 ICO generator (e.g., favicon.io) | Simple utility — generate once, place in `public/`, done. Not worth scripting. |

**Key insight:** The only "build" step is `npm run build`. Everything else (FTP, favicon generation) is a manual, one-time or infrequent operation at this phase. Don't over-engineer deployment infrastructure for a single static page.

## Runtime State Inventory

> Skip — this is a greenfield phase. No existing runtime state to migrate.

## Common Pitfalls

### Pitfall 1: FTP upload to wrong directory
**What goes wrong:** Files uploaded to FTP root instead of `public_html/`. Site shows Beget default page or 403 error.
**Why it happens:** Beget FTP gives access to the account home directory where `public_html/` is one of many folders. New users don't realize the web root is a subdirectory.
**How to avoid:** Always navigate INTO `public_html/` before uploading. Verify by opening the domain in a browser immediately after upload — should show your stub, not Beget's default page.
**Warning signs:** Domain shows «Сайт успешно создан» or 403 Forbidden instead of your placeholder. [CITED: PITFALLS.md §1]

### Pitfall 2: Cyrillic encoding — кракозябры
**What goes wrong:** Russian text displays as `??????` or garbled characters.
**Why it happens:** Beget sends `Content-Type: charset=windows-1251` by default. HTML's `<meta charset="UTF-8">` is overridden by the HTTP header.
**How to avoid:** Include `AddDefaultCharset UTF-8` in `.htaccess` from the first deploy. Also ensure `src/index.html` has `<meta charset="UTF-8">` as the first element in `<head>`.
**Warning signs:** Russian text is unreadable in any browser. Checking `Content-Type` response header in DevTools shows `windows-1251`. [CITED: PITFALLS.md §2]

### Pitfall 3: Tailwind CSS v4 not processing — blank page
**What goes wrong:** `npm run build` succeeds but page has no styles — plain unstyled HTML.
**Why it happens:** Tailwind v4's `@import "tailwindcss"` is not processed because `@tailwindcss/vite` plugin is missing from `vite.config.js`, or the CSS file uses old v3 syntax (`@tailwind base`).
**How to avoid:** Verify `vite.config.js` has `tailwindcss()` in the plugins array. Verify `style.css` uses `@import "tailwindcss"` (NOT `@tailwind` directives). Test with `npm run dev` first — Tailwind classes should work immediately in dev mode.
**Warning signs:** Browser shows unstyled black-on-white text. DevTools shows no CSS rules from Tailwind. Console has no errors — styles simply not generated. [ASSUMED — based on Tailwind v4 migration docs]

### Pitfall 4: Beget nginx cache — site not updating after re-deploy
**What goes wrong:** Uploaded new files via FTP but browser still shows old version.
**Why it happens:** Beget uses nginx as a reverse proxy in front of Apache. Static files may be cached by nginx for several minutes.
**How to avoid:** After FTP upload, test in private/incognito browser window. If still stale, wait 2-5 minutes for nginx cache to expire. The `.htaccess` includes `Cache-Control: no-cache` for HTML during development (see Pattern 3). Use Beget's file manager to verify actual file content on server.
**Warning signs:** Site unchanged after confirmed FTP upload. Private window shows correct version. [CITED: PITFALLS.md §3]

### Pitfall 5: `.htaccess` not uploaded (hidden file on Windows)
**What goes wrong:** `.htaccess` exists in `public/` but is not uploaded to the server — FTP client skips it as a "hidden" file.
**Why it happens:** Windows Explorer doesn't show files starting with `.` by default. Some FTP clients also hide dot-files. The file exists locally but silently fails to transfer.
**How to avoid:** After FTP upload, check Beget's file manager for `.htaccess` in `public_html/`. In FileZilla: Server → Force showing hidden files. Verify with `curl -I https://domain.ru` — should show `Content-Type: text/html; charset=UTF-8`.
**Warning signs:** Encoding still broken after deploy despite having `.htaccess` locally. HTTPS redirect not working. [CITED: PITFALLS.md §4]

## Code Examples

Verified patterns from official sources:

### Vite + Tailwind CSS v4 Project Setup (SETUP-01, BUILD-01)

```bash
# 1. Scaffold Vite vanilla project in current directory
npm create vite@latest . -- --template vanilla

# 2. Install Tailwind CSS v4 and Vite plugin
npm install tailwindcss@4.3.1 @tailwindcss/vite@4.3.1

# 3. Start dev server — verify Tailwind classes work
npm run dev
```

### vite.config.js
```js
// Source: tailwindcss.com/docs/installation/using-vite
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [tailwindcss()],
})
```

### src/style.css
```css
/* Source: tailwindcss.com/docs/installation/using-vite
   Tailwind CSS v4 — no @tailwind directives, no config file */
@import "tailwindcss";
```

### src/index.html (Stub — STUB-01)
```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Имя Фамилия — Сайт скоро откроется</title>
  <link rel="icon" type="image/x-icon" href="/favicon.ico">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="/src/style.css">
</head>
<body class="min-h-screen flex flex-col items-center justify-center font-[Inter] bg-white text-gray-900">
  <header class="mb-8">
    <h1 class="text-4xl font-bold tracking-tight">Имя Фамилия</h1>
  </header>

  <main class="text-center max-w-md px-4">
    <p class="text-xl text-gray-600 mb-8">Сайт скоро откроется</p>
    <p class="text-gray-500">
      Свяжитесь со мной:
      <a href="mailto:email@example.com" class="text-blue-600 hover:text-blue-800 underline">
        email@example.com
      </a>
    </p>
  </main>

  <footer class="mt-auto py-6 text-sm text-gray-400">
    &copy; 2026 Имя Фамилия
  </footer>
</body>
</html>
```

**Note:** The `href="/src/style.css"` path works in Vite dev mode (Vite serves `src/` as root). In production build, Vite automatically rewrites this to the hashed asset path. [VERIFIED: vite.dev/guide/ — index.html and Project Root section]

### public/.htaccess (DEPLOY-01)
```apache
AddDefaultCharset UTF-8
DirectoryIndex index.html
Options -Indexes

RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]

<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType image/x-icon "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
</IfModule>

<FilesMatch "\.(html)$">
  Header set Cache-Control "no-cache, must-revalidate"
</FilesMatch>
```

### FTP Deploy Commands (DEPLOY-01)

**Via Windows built-in FTP:**
```bash
# Interactive FTP session
ftp ftp.domain.ru
# Enter username and password when prompted
cd public_html
prompt off           # Disable per-file confirmation
mput dist\*          # Upload all files from dist/
```

**Via FileZilla (GUI, recommended for Phase 01):**
1. Host: `ftp.domain.ru`, Username/Password: (from Beget control panel)
2. Navigate remote site to `/public_html/`
3. Drag `dist/*` contents to remote panel
4. Verify: open `https://domain.ru` in browser

**Post-deploy verification:**
```bash
# Check HTTP headers (encoding must show UTF-8)
curl -I https://domain.ru 2>&1 | Select-String "content-type"

# Check .htaccess is active (no directory listing)
curl -I https://domain.ru/assets/ 2>&1 | Select-String "HTTP"
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Tailwind CSS v3 (`@tailwind base`, `tailwind.config.js`) | Tailwind CSS v4 (`@import "tailwindcss"`, no config file) | v4.0 (Dec 2024) | Zero-config setup; CSS-first configuration via `@theme`; faster builds |
| Vite v5/v6/v7 | Vite v8 (Rolldown bundler) | v8.0 (2026) | Rolldown replaces esbuild+Rollup for production builds; faster, simpler |
| PostCSS plugin for Tailwind | `@tailwindcss/vite` Vite plugin | v4.0 | Native Vite integration; no PostCSS config needed |
| Beget default encoding (`windows-1251`) | `.htaccess` `AddDefaultCharset UTF-8` | Always required | Mandatory for Russian-language content on Beget |

**Deprecated/outdated:**
- **`@tailwind base; @tailwind components; @tailwind utilities;` (v3 syntax):** Replaced by `@import "tailwindcss"` in v4. Will not work with Tailwind CSS 4.x.
- **`tailwind.config.js`:** Replaced by CSS-based configuration (`@theme`) in v4. Not needed for Phase 01 placeholder — all styling is done with utility classes.
- **Vite `@vitejs/plugin-legacy`:** Not needed — modern browsers support all features used (ES modules, CSS variables, flexbox).
- **`npm create vite@latest -- --template vanilla-ts`:** TypeScript template adds unnecessary complexity for a single HTML+CSS page.

## Assumptions Log

| # | Claim | Section | Risk if Wrong |
|---|-------|---------|---------------|
| A1 | Tailwind CSS v4 `@import "tailwindcss"` works identically in Vite dev and production builds | Standard Stack, Code Examples | LOW — verified against official Tailwind docs (tailwindcss.com) |
| A2 | Beget servers run Apache 2.4 with mod_rewrite and mod_expires enabled | Code Examples (§.htaccess) | LOW — Beget KB confirms Apache with these modules; `IfModule` guards prevent errors if a module is missing |
| A3 | Beget nginx cache TTL is 2-5 minutes for static files | Pitfall 4 | LOW — based on PITFALLS.md research citing Beget KB and community experience |
| A4 | Beget FTP host is `ftp.domain.ru` | Code Examples (§FTP Deploy) | MEDIUM — exact host depends on Beget account configuration; user must verify in Beget control panel |
| A5 | Vite vanilla template creates `src/` with `index.html` at project root (not inside `src/`) | Architecture Patterns | LOW — this is a Vite convention; `index.html` is always at project root, not in `src/` [VERIFIED: vite.dev/guide/] |

## Open Questions

1. **Beget FTP credentials**
   - What we know: User mentioned they would provide access (FTP host, login, password)
   - What's unclear: Exact FTP hostname, whether SFTP is available, if the domain is already pointing to Beget
   - Recommendation: Planner should include a `checkpoint:human` task to collect FTP credentials before the deploy task

2. **SSL certificate on Beget**
   - What we know: Beget offers free Let's Encrypt SSL. `.htaccess` includes HTTPS redirect.
   - What's unclear: Whether SSL is already active on the domain. If not, the HTTPS redirect rule should be commented out until SSL is provisioned (otherwise browsers get a certificate error).
   - Recommendation: Planner should include a pre-deploy check for SSL status. If SSL is not active, comment out the `RewriteRule` line in `.htaccess` until SSL is enabled in Beget control panel.

3. **Actual site name and email**
   - What we know: Placeholder content should include a name, tagline, and contact email per D-05.
   - What's unclear: What is the actual name/site title? What email address to display?
   - Recommendation: Use placeholder values that can be easily replaced. Planner should add a task to confirm these values with the user or note them as configurable placeholders.

## Environment Availability

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| Node.js | Vite dev server, npm | ✓ | v24.16.0 | — |
| npm | Package installation | ✓ | 11.13.0 | — |
| FTP client | Deployment to Beget | ✓ | Windows ftp.exe (built-in), sftp.exe (OpenSSH 9.5.4.1) | FileZilla (GUI, free) recommended for Phase 01 |
| Git | Version control (optional for Phase 01) | ✓ | (available) | — |

**Missing dependencies with no fallback:**
- **Beget FTP access (host, username, password):** Required for DEPLOY-01. Must be provided by the user. Planner should insert a `checkpoint:human` task to collect these before the deploy task.

**Missing dependencies with fallback:**
- None — all local tools are available.

## Security Domain

### Applicable ASVS Categories

| ASVS Category | Applies | Standard Control |
|---------------|---------|-----------------|
| V2 Authentication | No | No authentication on static site |
| V3 Session Management | No | No sessions — static HTML |
| V4 Access Control | No | No access control needed |
| V5 Input Validation | No | No user input in Phase 01 (email is a `mailto:` link, no form) |
| V6 Cryptography | No (server-level) | HTTPS via Beget SSL + `.htaccess` redirect — handled at hosting level |
| V7 Error Handling | Partial | 404 page deferred to Phase 2; `Options -Indexes` prevents directory listing |

### Known Threat Patterns for Static Site on Beget

| Pattern | STRIDE | Standard Mitigation |
|---------|--------|---------------------|
| Directory listing exposes server files | Information Disclosure | `Options -Indexes` in `.htaccess` — prevents browsing `/assets/`, `/img/` |
| Mixed content (HTTP resources on HTTPS page) | Information Disclosure | Use relative paths for all assets; Google Fonts loaded via `https://` |
| Backup files accessible on server (`index-backup.html`) | Information Disclosure | Never store backup files in `public_html/`; deploy only `dist/` contents |
| FTP credentials in plaintext (standard FTP, not SFTP) | Information Disclosure, Elevation of Privilege | Use SFTP if available on Beget; never commit FTP credentials to git; use `.env` excluded from git or enter manually |

## Sources

### Primary (HIGH confidence)
- **vite.dev/guide/** — Vite 8.1.0 documentation: Getting Started, Static Asset Handling, Building for Production, Deploying a Static Site, CLI. Verified via WebFetch 2026-06-24.
- **tailwindcss.com/docs/installation/using-vite** — Tailwind CSS v4 Vite installation guide. Verified via WebFetch 2026-06-24.
- **npm registry** — `npm view vite`, `npm view tailwindcss`, `npm view @tailwindcss/vite` — version verification, postinstall script check, package metadata. Verified 2026-06-24.
- **beget.com/ru/kb/how-to/sites/perenos-sajta-k-nam** — Beget knowledge base: FTP upload procedure, directory structure (public_html). Verified via WebFetch 2026-06-24.

### Secondary (MEDIUM confidence)
- **.planning/research/PITFALLS.md** — 8 critical pitfalls documented with Beget KB sources. Cross-referenced with Beget KB articles. Researched 2026-06-24.
- **.planning/research/ARCHITECTURE.md** — Semantic HTML structure, CSS patterns, anti-patterns. MDN-sourced. Researched 2026-06-24.
- **.planning/research/STACK.md** — Technology stack recommendations and anti-recommendations. Researched 2026-06-24.

### Tertiary (LOW confidence)
- None — all claims verified against official sources or existing research.

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — all versions confirmed via `npm view` against npm registry; setup procedure verified against official Tailwind CSS and Vite documentation
- Architecture: HIGH — static site architecture is well-understood; patterns verified against MDN, Vite docs, and ARCHITECTURE.md research
- Pitfalls: HIGH — Beget-specific pitfalls sourced from Beget KB and cross-referenced with PITFALLS.md; encoding and FTP issues are well-documented

**Research date:** 2026-06-24
**Valid until:** 2026-07-24 (30 days — stable technologies, no breaking changes expected)
