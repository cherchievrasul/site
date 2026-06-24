# Requirements: Персональный сайт

**Defined:** 2026-06-24
**Core Value:** Сайт доступен в интернете по своему домену — люди могут перейти по ссылке и увидеть страницу.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Заглушка (Placeholder)

- [ ] **PLACE-01**: Посетитель видит заглушку с именем/названием при переходе по домену
- [ ] **PLACE-02**: На заглушке указан способ связи (email или соцсеть)
- [ ] **PLACE-03**: Заглушка корректно отображается на мобильных (320px+) и десктопе
- [ ] **PLACE-04**: Русский текст отображается без кракозябр (UTF-8 + .htaccess)
- [ ] **PLACE-05**: Страница содержит favicon и базовые SEO-метатеги (title, description)

### Деплой (Deploy)

- [ ] **DEPL-01**: Сайт задеплоен на beget в директорию public_html/
- [ ] **DEPL-02**: Сайт открывается по домену через HTTPS
- [ ] **DEPL-03**: Настроен .htaccess (кодировка UTF-8, HTTPS-редирект, отключён листинг директорий)

### UI / Дизайн

- [ ] **UI-01**: Страница имеет визуальное оформление — шрифты, цвета, отступы
- [ ] **UI-02**: Добавлены секции: about, photo, social-ссылки
- [ ] **UI-03**: Страница выглядит законченной, а не черновиком

## v2 Requirements

Deferred to future release.

### Блог

- **BLOG-01**: Отдельная страница или секция со списком постов
- **BLOG-02**: RSS-лента для подписчиков
- **BLOG-03**: Каждый пост имеет дату, заголовок и URL

### Портфолио

- **PORT-01**: Секция или страница с примерами работ
- **PORT-02**: Каждый проект имеет скриншот, описание и ссылку

## Out of Scope

| Feature | Reason |
|---------|--------|
| CMS / админ-панель | Избыточно для сайта-визитки |
| Бэкенд / серверная логика | Статический сайт — не нужен |
| База данных | Хранение только в HTML |
| Комментарии | Нужен бэкенд или сторонний сервис |
| Личный кабинет / регистрация | Не соответствует концепции сайта-визитки |
| Реактивные фреймворки (React/Vue) | Избыточно — статический HTML/CSS |
| OAuth / сторонняя авторизация | Нет закрытых разделов |
| Многостраничная архитектура | Фаза 1 — одностраничный сайт |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| PLACE-01 | Phase 1 | Pending |
| PLACE-02 | Phase 1 | Pending |
| PLACE-03 | Phase 1 | Pending |
| PLACE-04 | Phase 1 | Pending |
| PLACE-05 | Phase 1 | Pending |
| DEPL-01 | Phase 1 | Pending |
| DEPL-02 | Phase 1 | Pending |
| DEPL-03 | Phase 1 | Pending |
| UI-01 | Phase 2 | Pending |
| UI-02 | Phase 2 | Pending |
| UI-03 | Phase 2 | Pending |

**Coverage:**
- v1 requirements: 11 total
- Mapped to phases: 11
- Unmapped: 0 ✓

---
*Requirements defined: 2026-06-24*
*Last updated: 2026-06-24 after initial definition*
