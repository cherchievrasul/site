# Phase 1: Заглушка + Деплой - Context

**Gathered:** 2026-06-24
**Status:** Ready for planning

<domain>
## Phase Boundary

Сайт доступен по домену с оформленной заглушкой и проверенным пайплайном деплоя. Посетитель видит страницу с именем, сообщением «скоро открытие» и контактом. Деплой через FTP на beget в `public_html/`.

</domain>

<decisions>
## Implementation Decisions

### Стек и инструменты
- **D-01:** Vite 8 (vanilla template) + Tailwind CSS 4 — build tool и стили
- **D-02:** Tailwind подключается через `@tailwindcss/vite` плагин, CSS через `@import "tailwindcss"` (v4 синтаксис, без tailwind.config.js)
- **D-03:** Шрифт Inter через Google Fonts CDN (один `<link>` в `<head>`)
- **D-04:** Никакого JavaScript на заглушке — чистый HTML + CSS

### Контент заглушки
- **D-05:** Заглушка содержит: имя/название, теглайн «Сайт скоро откроется», способ связи (email)
- **D-06:** Вся страница — один `index.html`. Секции: header (название), main (сообщение + контакт), footer (копирайт)

### Структура проекта
```
project/
  src/
    index.html
    style.css
  dist/          ← build output
  .htaccess      ← в dist/ для деплоя
  package.json
  vite.config.js
```

### Деплой
- **D-07:** Ручной FTP-деплой: `npm run build` → загрузка `dist/*` в `public_html/` на beget
- **D-08:** `.htaccess` в корне сайта: `AddDefaultCharset UTF-8`, HTTPS-редирект, `Options -Indexes`

### Claude's Discretion
- Точные отступы и размеры шрифтов
- Цветовая схема (тёмный текст на светлом фоне, один акцентный цвет)
- Выравнивание — горизонтально и вертикально по центру
- Структура `.htaccess` — стандартная для Apache/beget

</decisions>

<canonical_refs>
## Canonical References

### Проект
- `.planning/PROJECT.md` — Контекст проекта, ограничения (beget, статика, без JS на этапе заглушки)
- `.planning/REQUIREMENTS.md` §v1 — Требования SETUP-01, BUILD-01, DEPLOY-01, STUB-01
- `.planning/ROADMAP.md` §Phase 1 — Цель фазы и success criteria

### Исследование
- `.planning/research/STACK.md` — Рекомендованный стек (Vite + Tailwind), анти-рекомендации
- `.planning/research/ARCHITECTURE.md` — HTML-структура, семантические секции, mobile-first CSS
- `.planning/research/PITFALLS.md` — Критические ошибки: FTP-директория (§1), кодировка (§2), .htaccess (§3), кэширование (§5)

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- Нет — greenfield проект, код создаётся с нуля

### Established Patterns
- Vite vanilla template как основа — минимум бойлерплейта
- Tailwind CSS v4 с `@import "tailwindcss"` — без файлов конфигурации

### Integration Points
- `dist/` → FTP → `public_html/` на beget
- `.htaccess` должен быть включён в `dist/` при сборке (скопирован или лежит в `public/`)

</code_context>

<specifics>
## Specific Ideas

- Пользователь явно хочет заглушку ДО UI — не нужно делать красивый дизайн в первой фазе
- Заглушка должна выглядеть прилично, но минималистично — «сайт в разработке» с именем и контактом
- Пользователь предлагал предоставить доступы к beget — нужны будут на этапе деплоя: FTP-хост, логин, пароль

</specifics>

<deferred>
## Deferred Ideas

- Автоматический деплой через GitHub Actions — Phase 2+
- Тёмная тема — Phase 3
- Форма связи — Phase 3
- Красивый UI/дизайн — Phase 2
- Фото, about, портфолио — Phase 2

</deferred>

---

*Phase: 01-placeholder*
*Context gathered: 2026-06-24*
