# Features Research

**Domain:** Personal static website / landing page
**Researched:** 2026-06-24
**Confidence:** HIGH

## Feature Categories

### Table Stakes (Must Have — v1)

| Feature | Complexity | Description |
|---------|-----------|-------------|
| Hero section | LOW | Name/title, tagline, call-to-action. Visitor instantly knows who and what. |
| Responsive design | MEDIUM | Mobile-first (320px+), breakpoints at 640/768/1024px. Must work on real devices. |
| About / Bio | LOW | 2-3 sentences about the person. Optional photo. |
| Contact info | LOW | Email or social link. Non-negotiable — site without contacts is broken. |
| Navigation | LOW | Anchor links to sections. Sticky header on scroll. |
| Profile photo | LOW | Single image, optimized (<200 KB), WebP format. |
| Social links | LOW | Icons linking to profiles. Lucide icons, row layout. |
| Favicon + SEO | LOW | `<title>`, `<meta description>`, `og:image`, favicon.ico. |
| Footer | LOW | Copyright, repeat contact link. |

### Near Table Stakes (v1 Optional)

| Feature | Complexity | Description |
|---------|-----------|-------------|
| Portfolio showcase | MEDIUM | Projects with screenshots, descriptions, links. |
| Resume section | LOW-MEDIUM | Work experience, education, skills. Timeline layout. |
| Custom 404 page | LOW | Friendly error page with link back to home. |

### Differentiators (v2)

| Feature | Complexity | Differentiation | Description |
|---------|-----------|-----------------|-------------|
| Dark/light theme | LOW-MEDIUM | HIGH | ~50 lines of JS + CSS. Respects `prefers-color-scheme`. |
| Contact form | MEDIUM | MEDIUM | Formspree or EmailJS — no backend needed. PHP fallback available. |
| Blog with RSS | MEDIUM | MEDIUM | Only if content commitment exists. SSG or manual HTML. |
| Scroll animations | LOW | LOW | Intersection Observer, CSS transitions. |
| Accessibility (WCAG AA) | MEDIUM | HIGH | Semantic HTML, ARIA labels, keyboard nav, contrast ratios. |
| Analytics | LOW | LOW | Plausible or Umami — privacy-first, lightweight. |
| Structured data (JSON-LD) | LOW | LOW | Rich snippets in search results. |

### Anti-Features (Deliberately NOT Building)

| Feature | Reason |
|---------|--------|
| User accounts / auth | No private content, no user data |
| Comments | Requires backend or third-party — not needed for personal site |
| CMS | Over-engineering for static content |
| Real-time chat | Wrong product category |
| E-commerce / payments | Out of scope |
| Multi-language (i18n) | Russian-only, YAGNI |
| Search | Full-text search for 1-5 pages is absurd |

## MVP Definition (Phase 1)

**Goal:** Live placeholder page on the domain — visitor sees WHO, WHAT, and HOW TO CONTACT.

**Included:** All 9 table stakes.
**Excluded:** Everything else — defer to Phase 2+.
