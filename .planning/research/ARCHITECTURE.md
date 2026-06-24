# Architecture Research

**Domain:** Personal static website (визитка / landing page) on shared hosting
**Researched:** 2026-06-24
**Confidence:** HIGH

## Standard Architecture

### System Overview

This is a **pure static website** — no backend, no database, no JavaScript at the initial stage. The architecture is a flat file tree served by a standard web server (beget shared hosting). Every "component" is an HTML semantic section styled by CSS.

```
┌─────────────────────────────────────────────────────────────┐
│                      Client (Browser)                        │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Browser  │  │  HTML    │  │   CSS    │  │  Fonts/  │   │
│  │  Cache   │  │  Parser  │  │  Engine  │  │  Images  │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
│       │              │              │              │         │
├───────┴──────────────┴──────────────┴──────────────┴─────────┤
│                         HTTP/HTTPS                           │
├─────────────────────────────────────────────────────────────┤
│                    Hosting (beget)                            │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                 Static File Server                     │   │
│  │  index.html  →  style.css  →  img/*  →  404.html     │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Key insight:** For a static site of this scale, the architecture is primarily about **file organization** and **HTML semantic structure**. There are no runtime components — everything is a flat file served directly.

### Component Boundaries (HTML Semantic Structure)

The page is decomposed into semantic HTML5 sections. Each section is a conceptual "component" that owns its content and is styled independently.

```
┌─────────────────────────────────────────────────┐
│                   <header>                       │
│  ┌─────────────────────────────────────────┐    │
│  │  Site name / logo    Language (ru)       │    │
│  └─────────────────────────────────────────┘    │
├─────────────────────────────────────────────────┤
│              <main>                              │
│  ┌─────────────────────────────────────────┐    │
│  │  <section id="hero">                    │    │
│  │    Name, tagline, CTA                   │    │
│  │  </section>                             │    │
│  ├─────────────────────────────────────────┤    │
│  │  <section id="about">                   │    │
│  │    Bio, description, photo              │    │
│  │  </section>                             │    │
│  ├─────────────────────────────────────────┤    │
│  │  <section id="contacts">                │    │
│  │    Email, Telegram, social links        │    │
│  │  </section>                             │    │
│  └─────────────────────────────────────────┘    │
├─────────────────────────────────────────────────┤
│                   <footer>                       │
│  ┌─────────────────────────────────────────┐    │
│  │  Copyright, year, attribution            │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| `<header>` | Site identity — name, logo, optional navigation | Fixed/sticky bar at top, minimal |
| `<section id="hero">` | First impression — name, tagline, what this site is | Full-viewport hero with centered text |
| `<section id="about">` | Who this person is — brief bio, background | Text block with optional image |
| `<section id="contacts">` | How to reach — email, social links, Telegram | Icon list or link row |
| `<footer>` | Legal/bottom info — copyright, year | Thin bar at page bottom |
| `style.css` | All visual presentation — layout, colors, typography, responsive | Single CSS file organized with `@layer` |
| `img/` | Static assets — photos, icons, favicon | Flat folder of optimized images |

**Boundary principle:** Each `<section>` is self-contained. Content inside one section does not depend on or leak into another section. This means sections can be added, removed, or reordered without breaking others.

## Recommended Project Structure

```
sait/                          # Project root (deploys as-is to hosting)
├── index.html                 # Main page — the only page in Phase 1
├── 404.html                   # Custom 404 (optional but good practice)
├── css/
│   └── style.css              # All styles in one file (Phases 1-2)
├── img/
│   ├── favicon.ico            # Browser tab icon
│   ├── photo.jpg              # Personal photo (if used)
│   └── og-image.jpg           # Social media preview image
├── .htaccess                  # Apache config (beget uses Apache)
└── README.md                  # Deployment notes (not uploaded to hosting)
```

**Future evolution (Phase 2+):**
```
sait/
├── index.html                 # Same structure, enhanced
├── css/
│   ├── reset.css              # CSS reset (extracted from style.css)
│   ├── base.css               # Typography, colors, variables
│   ├── layout.css             # Page structure, responsive
│   └── components.css         # Reusable patterns
├── img/
│   └── ...                    # More images as site grows
├── pages/
│   ├── about.html             # Expanded about page
│   ├── portfolio.html         # Work/portfolio page
│   └── blog.html              # Blog (if added later)
└── js/
    └── main.js                # JavaScript (only when needed, Phase 3+)
```

### Structure Rationale

- **Flat at root:** Hosting (beget) serves files from root. `index.html` is the default page. No nesting needed for a single-page site.
- **`css/` folder:** Separates presentation from content. Single file (`style.css`) is appropriate for Phase 1 — splitting into multiple CSS files requires a build step or `@import`, which adds latency.
- **`img/` folder:** All binary assets in one place. Prevents clutter at root level.
- **`.htaccess`:** Beget runs Apache. Useful for redirects, caching headers, custom error pages.
- **No `node_modules/`, no `package.json`:** No build tools needed. This is plain HTML+CSS edited directly and uploaded via FTP.

## Architectural Patterns

### Pattern 1: Semantic Sectioning

**What:** Use HTML5 semantic elements (`<header>`, `<main>`, `<section>`, `<footer>`) instead of generic `<div>` wrappers. Each section has a unique `id` for navigation and an optional `class` for styling.

**When to use:** Always — this is the standard for modern HTML. It improves accessibility (screen readers can navigate by landmark), SEO (search engines understand page structure), and maintainability (clear boundaries between content areas).

**Trade-offs:** None for this project scale. Semantic elements are widely supported in all modern browsers.

**Example:**
```html
<header>
  <h1>Иван Иванов</h1>
  <p class="tagline">Веб-разработчик</p>
</header>

<main>
  <section id="hero">
    <h2>Привет!</h2>
    <p>Я создаю сайты, которые работают.</p>
  </section>

  <section id="about">
    <h2>Обо мне</h2>
    <p>5 лет опыта в веб-разработке...</p>
  </section>

  <section id="contacts">
    <h2>Контакты</h2>
    <a href="mailto:ivan@example.com">Email</a>
    <a href="https://t.me/ivan">Telegram</a>
  </section>
</main>

<footer>
  <p>&copy; 2026 Иван Иванов</p>
</footer>
```

### Pattern 2: CSS Cascade Layers (`@layer`)

**What:** Use CSS `@layer` to define a clear priority order for styles. Layers resolve specificity conflicts without `!important` hacks. The order: `reset → base → layout → components → utilities`.

**When to use:** As soon as the stylesheet grows beyond ~100 lines (likely Phase 2). For Phase 1, simple selectors without `@layer` are sufficient. Introducing `@layer` early prevents specificity wars later.

**Trade-offs:** Adds conceptual overhead for very small stylesheets. Not needed if the project stays under 200 lines of CSS. But adding it later means refactoring — better to start with it.

**Example:**
```css
@layer reset, base, layout, components, utilities;

/* Layer 1: Normalize browser defaults */
@layer reset {
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  img { display: block; max-width: 100%; }
}

/* Layer 2: Typography, colors, variables */
@layer base {
  :root {
    --color-text: #1a1a1a;
    --color-bg: #ffffff;
    --color-accent: #2563eb;
  }
  body {
    font-family: system-ui, -apple-system, sans-serif;
    color: var(--color-text);
    background: var(--color-bg);
    line-height: 1.6;
  }
  h1, h2, h3 { line-height: 1.2; }
}

/* Layer 3: Page-level layout */
@layer layout {
  header { padding: 2rem 1rem; text-align: center; }
  main > section { padding: 4rem 1rem; max-width: 48rem; margin: 0 auto; }
}

/* Layer 4: Reusable components */
@layer components {
  .btn { display: inline-block; padding: 0.75rem 1.5rem; background: var(--color-accent); color: white; border-radius: 0.5rem; text-decoration: none; }
}

/* Layer 5: One-off overrides */
@layer utilities {
  .text-center { text-align: center; }
}
```

### Pattern 3: Mobile-First Responsive Layout

**What:** Write base styles for mobile (narrow viewport), then use `min-width` media queries to add complexity for wider screens. This is the standard responsive web design approach.

**When to use:** From day one. The project requirement is "корректно отображается на мобильных и десктопе" — responsive is mandatory, not optional.

**Trade-offs:** Mobile-first means the base styles are for small screens, which can feel weird if you primarily design on desktop. But it prevents the common mistake of building desktop-only and trying to "squeeze" it down.

**Example:**
```css
/* Mobile base (no media query — this is the default) */
#contacts a {
  display: block;        /* Stack vertically on mobile */
  padding: 0.75rem;
}

/* Tablet and up */
@media (min-width: 640px) {
  #contacts a {
    display: inline-block; /* Side by side on wider screens */
  }
}

/* Desktop */
@media (min-width: 1024px) {
  main > section {
    padding: 6rem 2rem;
  }
}
```

### Pattern 4: Single-File CSS (Phase 1) → Split CSS (Phase 2)

**What:** Start with one `style.css` file. When it exceeds ~300 lines or becomes hard to navigate, split into logical files: `reset.css`, `base.css`, `layout.css`, `components.css`. Use `@import` or multiple `<link>` tags in HTML.

**When to use:** Phase 1: single file. Phase 2: split when complexity warrants it.

**Trade-offs:** Multiple files = more HTTP requests (minor concern on modern HTTP/2). Single file = harder to navigate as it grows. For beget shared hosting, assume HTTP/1.1 — keep file count low in Phase 1.

**Example (Phase 2 HTML with split CSS):**
```html
<link rel="stylesheet" href="css/reset.css">
<link rel="stylesheet" href="css/base.css">
<link rel="stylesheet" href="css/layout.css">
<link rel="stylesheet" href="css/components.css">
```

## Data Flow

### Request Flow (the only flow that matters)

```
User types URL (https://mysite.ru)
    ↓
DNS resolves to beget server IP
    ↓
Browser sends HTTP GET / request
    ↓
Apache on beget serves index.html
    ↓
Browser parses HTML
    ├── Encounters <link rel="stylesheet" href="css/style.css">
    │   ↓
    │   Browser sends HTTP GET /css/style.css
    │   ↓
    │   Apache serves style.css
    │   ↓
    │   CSS engine applies styles to DOM
    │
    ├── Encounters <img src="img/photo.jpg">
    │   ↓
    │   Browser sends HTTP GET /img/photo.jpg
    │   ↓
    │   Apache serves photo.jpg
    │   ↓
    │   Image renders in layout
    │
    └── Browser finishes rendering page
        ↓
    User sees the website
```

### Content Flow (how information is structured)

For a static site, "data" is not processed at runtime — it's authored directly in HTML. The flow is about **how content is organized**, not how it's fetched:

```
Author writes content
    ↓
Content lives in index.html sections:
    ├── <header> → Site identity (name, tagline)
    ├── <section id="hero"> → Value proposition (who, what)
    ├── <section id="about"> → Background (bio, experience)
    ├── <section id="contacts"> → Contact channels (email, social)
    └── <footer> → Legal/copyright
    ↓
CSS in style.css provides:
    ├── Typography (fonts, sizes, hierarchy)
    ├── Colors (brand palette, contrast)
    ├── Layout (responsive grid, spacing)
    └── Visual polish (shadows, borders, transitions)
    ↓
Browser renders the composed page
```

**Critical insight:** There is no runtime state. No JavaScript. No data fetching. Every byte of content is in `index.html`. This is not a limitation — it's the correct architecture for a placeholder/landing page. It's fast, reliable, and works on any browser.

### Phase 1 vs Phase 2 Data Flow

| Aspect | Phase 1 (Placeholder) | Phase 2 (Full Site) |
|--------|----------------------|---------------------|
| Pages | Single `index.html` | Multiple HTML pages |
| Content source | Hardcoded in HTML | Hardcoded in HTML (still static) |
| Navigation | None (single page) | `<nav>` with links between pages |
| Images | 0-2 images | Multiple images, possibly optimized |
| External resources | 0 (no external fonts/CDN) | Possibly Google Fonts or self-hosted fonts |
| JavaScript | None | Possibly light JS for interactions |

## Scaling Considerations

For a personal static website, "scaling" doesn't mean millions of users — it means **content growth** and **maintainability**.

| Scale | Architecture Adjustments |
|-------|--------------------------|
| **Phase 1: Placeholder** (1 page) | Single `index.html` + single `style.css`. Flat structure. All content inline. |
| **Phase 2: Full site** (3-10 pages) | Split CSS into logical files. Add `pages/` folder. Add `<nav>` navigation. Possibly add shared header/footer via PHP includes (beget supports PHP). |
| **Phase 3+: Blog/portfolio** (10+ pages) | Consider static site generator (Hugo, Eleventy) if content volume justifies it. Or use PHP includes on beget to avoid duplicating header/footer. |

### Scaling Priorities

1. **First growing pain:** Duplicating `<header>` and `<footer>` across multiple HTML pages. **Fix:** Use PHP `<?php include 'header.php'; ?>` (beget supports PHP). No framework needed — PHP is available on the hosting and can be used minimally for templating.
2. **Second growing pain:** Managing CSS across many pages. **Fix:** Already addressed by `@layer` architecture. Refactor shared styles into base/components layers, page-specific styles into the page's own stylesheet.

## Anti-Patterns

### Anti-Pattern 1: `<div>` Soup Instead of Semantic HTML

**What people do:** Wrap everything in `<div class="header">`, `<div class="nav">`, `<div class="footer">` instead of using `<header>`, `<nav>`, `<footer>`.

**Why it's wrong:** Screen readers can't navigate. Search engines can't understand page structure. CSS selectors become needlessly specific (`.header .header-inner .header-title` vs `header h1`).

**Do this instead:** Use semantic HTML5 elements. They work in all browsers and have zero cost.

### Anti-Pattern 2: Desktop-First CSS

**What people do:** Write CSS for wide screens first, then use `max-width` media queries to "fix" mobile. This is backwards.

**Why it's wrong:** Mobile ends up with override-over-override chains. Performance suffers on mobile (where it matters most). The page breaks in unexpected ways at intermediate widths.

**Do this instead:** Write mobile styles as the base (no media query), then use `min-width` to enhance for larger screens.

### Anti-Pattern 3: Inline Styles for Everything

**What people do:** Put `style="color: red; font-size: 20px"` directly on HTML elements, especially "just for this one section."

**Why it's wrong:** Inline styles can't be overridden by CSS (except with `!important`). They make the design inconsistent. They bloat HTML and can't be cached separately.

**Do this instead:** All styles in `style.css`. Use descriptive class names or semantic selectors.

### Anti-Pattern 4: Adding a Framework "Just in Case"

**What people do:** Add React, Tailwind, or Bootstrap to a 1-page static site "because we might need it later."

**Why it's wrong:** Adds 50-200KB of JavaScript/CSS for a site that needs 0KB. Increases complexity, build steps, and deployment friction. Violates the constraint "статический сайт без бэкенда" and the decision "отказ от CMS/фреймворков."

**Do this instead:** Plain HTML + CSS. Add tools only when the pain of not having them exceeds the pain of having them. For a personal landing page, that threshold is never reached.

### Anti-Pattern 5: Giant Hero Images on Mobile

**What people do:** Use a 4000px-wide background image for the hero section that downloads on mobile.

**Why it's wrong:** 3-5MB download on a mobile connection. Slow Largest Contentful Paint (LCP). Users bounce before the page loads.

**Do this instead:** CSS-only hero (gradients, solid colors, subtle patterns). If images are absolutely needed, use responsive images (`<img srcset>` or `<picture>`) with properly sized versions.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| Beget hosting | FTP upload or file manager | Deploy by copying files to `public_html/` on the server |
| Domain (beget) | DNS A-record pointing to hosting IP | Already configured per project context |
| Email (contact) | `mailto:` link in HTML | No server-side form needed for Phase 1 |
| Telegram | `https://t.me/username` link | Static link, no API integration |
| Social links | Plain `<a href>` links | No embedded widgets (they add JS bloat) |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| `index.html` ↔ `style.css` | HTML `<link>` tag | Standard CSS loading. Ensure path is relative (`css/style.css`) |
| `index.html` ↔ `img/*` | HTML `<img>` tags | Standard image loading. Always set `alt` attributes |
| Section ↔ Section | None (sibling DOM nodes) | Sections are independent. No data sharing between them |
| Page ↔ Page (Phase 2+) | HTML `<a href>` links | Standard navigation. No SPA routing |

### .htaccess Configuration (for beget Apache hosting)

```apache
# Force HTTPS (good practice)
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]

# Custom 404 page
ErrorDocument 404 /404.html

# Browser caching for static assets
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/x-icon "access plus 1 year"
</IfModule>
```

## Build Order Implications (from Architecture)

The architecture directly informs the build order. Dependencies flow in one direction:

```
Phase 1: index.html (structure)
    ↓ (depends on)
Phase 1: style.css (presentation)
    ↓ (reuses same architecture)
Phase 2: Enhanced CSS (split files, @layer, refined design)
    ↓ (extends with)
Phase 2: Additional pages (about, portfolio — same section pattern)
    ↓ (optional, only if needed)
Phase 3: JavaScript (light interactivity — form validation, animations)
```

**Critical dependency chain:**
1. HTML structure must exist before CSS can style it
2. CSS reset/base must exist before component styles
3. Responsive layout must work on mobile before desktop enhancements
4. Single page must work before adding more pages

**Recommended build order within Phase 1:**
1. `index.html` — semantic skeleton with all content (Russian text)
2. `style.css` — mobile-first base styles (typography, colors, spacing)
3. Responsive breakpoints — tablet and desktop media queries
4. Visual polish — shadows, transitions, hover states
5. `img/` — favicon, any photos, social preview image
6. `.htaccess` — HTTPS redirect, caching, 404
7. `404.html` — simple error page

Each step produces a working page. You can stop at any step and deploy — the site is functional.

## Sources

- MDN Web Docs — "Structuring documents" (HTML semantic sections, layout elements). https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents — **HIGH confidence** (official web standards documentation, last modified Aug 2025)
- Matt Cone — "The Static Site Guide" (static site directory structure, deployment). Context7 library `/mattcone/static-guide` — **HIGH confidence** (published guide, GitHub-hosted)
- Modern CSS — "CSS Cascade Layers" (`@layer` architecture for organizing styles). Context7 library `/websites/modern-css` — **MEDIUM confidence** (community resource, contemporary patterns)
- web.dev — "Learn Responsive Design" (mobile-first, fluid grids, viewport meta). https://web.dev/learn/design/intro/ — **HIGH confidence** (Google-authored web education, last updated Nov 2021)
- Project constraints from `.planning/PROJECT.md` — beget hosting, static HTML+CSS, no JS initially, Russian language — **HIGH confidence** (project source of truth)

---

*Architecture research for: personal static website (персональный сайт-визитка)*
*Researched: 2026-06-24*
