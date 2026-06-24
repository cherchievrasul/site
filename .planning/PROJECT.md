# Персональный сайт

## What This Is

Персональный сайт-визитка на собственном домене (beget). Начинается с заглушки (landing-страницы), затем развивается до полноценного сайта с контентом и оформлением.

## Core Value

Сайт доступен в интернете по своему домену — люди могут перейти по ссылке и увидеть страницу.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Посетитель видит страницу-заглушку при переходе по домену
- [ ] Страница-заглушка содержит базовую информацию (название, контакты)
- [ ] Страница корректно отображается на мобильных и десктопе
- [ ] Сайт задеплоен на хостинг beget и работает по домену
- [ ] UI/дизайн сайта (после заглушки)

### Out of Scope

- Админ-панель / CMS — избыточно для v1
- Серверная логика / бэкенд — статический сайт
- База данных — не требуется

## Context

- Домен куплен на beget (российский хостинг-провайдер)
- Хостинг поддерживает статические файлы (HTML/CSS/JS) и PHP
- Проект начинается с нуля (greenfield)
- Пользователь хочет сначала заглушку, потом UI — пошаговый подход

## Constraints

- **Хостинг**: beget — деплой через FTP или файловый менеджер хостинга
- **Стек**: Статический сайт (HTML + CSS), без JavaScript на этапе заглушки
- **Язык**: Русский

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Статический сайт без бэкенда | beget поддерживает статику, для заглушки бэкенд не нужен | — Pending |
| Сначала заглушка, потом UI | Поэтапный подход, видимый результат с первого шага | — Pending |
| Отказ от CMS/фреймворков | Избыточно для персонального сайта-визитки | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-06-24 after initialization*
