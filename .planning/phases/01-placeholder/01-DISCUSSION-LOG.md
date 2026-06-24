# Phase 1: Заглушка + Деплой — Discussion Log

**Date:** 2026-06-24
**Mode:** auto
**Areas discussed:** All (auto-selected)

## Gray Areas Resolved

### Stack & Tools
- **Q:** Vite vanilla + Tailwind vs raw HTML?
- **A:** Vite vanilla + Tailwind CSS 4 (recommended by research — build tool with HMR, production minification, Tailwind utility classes)
- **[auto]** Selected: Vite + Tailwind (recommended default)

### Content
- **Q:** What goes on the placeholder?
- **A:** Name/title + "coming soon" tagline + email contact — minimal identity stub
- **[auto]** Selected: Minimal identity (recommended default)

### Styling
- **Q:** How styled should placeholder be?
- **A:** Clean centered layout, Inter font, Tailwind utilities — presentable but not over-designed
- **[auto]** Selected: Clean minimal (recommended default)

### Deployment
- **Q:** FTP manual vs automated?
- **A:** Manual FTP for Phase 1 — validate pipeline, automate later
- **[auto]** Selected: Manual FTP (recommended default)

### Server Config
- **Q:** .htaccess contents?
- **A:** UTF-8 encoding + HTTPS redirect + directory listing off per PITFALLS.md
- **[auto]** Selected: Full .htaccess from PITFALLS.md (recommended default)

## Deferred Ideas

- CI/CD auto-deploy (GitHub Actions) — Phase 2+
- Dark theme — Phase 3
- Contact form — Phase 3
- Full UI/design — Phase 2

---

*Discussion completed: 2026-06-24*
