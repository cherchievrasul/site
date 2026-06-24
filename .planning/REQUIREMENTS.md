# Requirements: Персональный сайт

**Defined:** 2026-06-24
**Core Value:** Сайт доступен в интернете по своему домену — люди могут перейти по ссылке и увидеть страницу.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Setup & Build (Phase 1)

- [ ] **SETUP-01**: Проект инициализирован — Vite vanilla + Tailwind CSS 4, dev-сервер работает локально
- [ ] **BUILD-01**: Команда `npm run build` собирает статические файлы в `dist/`

### Деплой (Phase 1)

- [ ] **DEPLOY-01**: Сайт задеплоен на beget в `public_html/`, открывается по домену через HTTPS
- [ ] **STUB-01**: Посетитель видит оформленную заглушку (имя, «скоро открытие», способ связи, favicon)

### Дизайн и контент (Phase 2)

- [ ] **HERO-01**: Hero-секция с именем, заголовком, фото и кратким описанием
- [ ] **NAV-01**: Навигация с якорными ссылками на секции, sticky header
- [ ] **ABOUT-01**: Секция «Обо мне» — 2-3 предложения, фото
- [ ] **PORT-01**: Секция портфолио — сетка карточек с проектами (скриншот, описание, ссылка)
- [ ] **CONTACT-01**: Секция контактов — email и ссылки на соцсети с иконками
- [ ] **SEO-01**: Метатеги title/description/og:image, favicon, иконки для iOS/Android
- [ ] **FOOT-01**: Подвал с копирайтом и ссылкой на контакты
- [ ] **RESP-01**: Сайт корректно отображается на 320px, 768px, 1024px, 1920px+
- [ ] **ERR-01**: Кастомная страница 404 в стиле сайта

### Полировка и интерактив (Phase 3)

- [ ] **THEME-01**: Переключатель тёмной/светлой темы, сохраняется между сессиями
- [ ] **FORM-01**: Форма связи отправляет сообщение (Formspree или аналог) с подтверждением
- [ ] **SKILL-01**: Секция навыков — список технологий/инструментов
- [ ] **RESUME-01**: Секция резюме с опытом работы и образованием, версия для печати
- [ ] **ANALYTICS-01**: Аналитика посещений (Plausible/Umami), без cookie-баннеров
- [ ] **ANIM-01**: Анимации появления секций при скролле (Intersection Observer)

## v2 Requirements

Deferred to future release.

### Блог

- **BLOG-01**: Секция со списком постов, каждый с датой и заголовком
- **BLOG-02**: RSS-лента
- **BLOG-03**: JSON-LD structured data для поисковиков

### Accessibility

- **A11Y-01**: WCAG AA аудит — контраст, keyboard nav, ARIA-метки
- **A11Y-02**: Screen-reader тестирование

### Расширенный контент

- **TEST-01**: Секция отзывов / testimonials
- **TIME-01**: Хронология опыта (timeline layout)

## Out of Scope

| Feature | Reason |
|---------|--------|
| CMS / админ-панель | Избыточно для сайта-визитки |
| Бэкенд / серверная логика | Статический сайт — не нужен |
| База данных | Хранение только в HTML |
| Комментарии | Нужен бэкенд или сторонний сервис |
| Личный кабинет / регистрация | Не соответствует концепции сайта-визитки |
| React / Vue / Svelte | Избыточно — статический HTML/CSS |
| OAuth / сторонняя авторизация | Нет закрытых разделов |
| Многостраничная архитектура (кроме 404) | Фазы 1-2 — одностраничный сайт |
| i18n / многоязычность | Русский — достаточно |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| SETUP-01 | Phase 1 | Pending |
| BUILD-01 | Phase 1 | Pending |
| DEPLOY-01 | Phase 1 | Pending |
| STUB-01 | Phase 1 | Pending |
| HERO-01 | Phase 2 | Pending |
| NAV-01 | Phase 2 | Pending |
| ABOUT-01 | Phase 2 | Pending |
| PORT-01 | Phase 2 | Pending |
| CONTACT-01 | Phase 2 | Pending |
| SEO-01 | Phase 2 | Pending |
| FOOT-01 | Phase 2 | Pending |
| RESP-01 | Phase 2 | Pending |
| ERR-01 | Phase 2 | Pending |
| THEME-01 | Phase 3 | Pending |
| FORM-01 | Phase 3 | Pending |
| SKILL-01 | Phase 3 | Pending |
| RESUME-01 | Phase 3 | Pending |
| ANALYTICS-01 | Phase 3 | Pending |
| ANIM-01 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 19 total
- Mapped to phases: 19
- Unmapped: 0 ✓

---
*Requirements defined: 2026-06-24*
*Last updated: 2026-06-24 after roadmap creation*
