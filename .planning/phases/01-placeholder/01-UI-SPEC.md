# UI-SPEC: Phase 01 — Заглушка

**Generated:** 2026-06-24
**Mode:** auto (placeholder — minimal design)

## Layout

### Structure
Одностраничный центрированный layout. Три семантические секции сверху вниз:
```
┌──────────────────────────┐
│         Header           │  ← название / логотип-текст
├──────────────────────────┤
│          Main            │  ← основное сообщение + контакт
│                          │
│   (центрировано по       │
│    вертикали и горизонтали)│
│                          │
├──────────────────────────┤
│         Footer           │  ← копирайт
└──────────────────────────┘
```

### Dimensions
- `min-h-screen` — вся высота viewport
- `flex flex-col items-center justify-center` — центрирование по обеим осям
- `max-w-prose` на основном блоке — комфортная ширина чтения (~65ch)
- Паддинги: `p-6 sm:p-8 md:p-12`

## Typography

### Font Family
- **Primary:** Inter (Google Fonts CDN, weights 400, 600, 700)
- **Fallback:** `system-ui, -apple-system, sans-serif`

### Font Sizes (Tailwind classes)
| Элемент | Размер | Weight |
|---------|--------|--------|
| Заголовок (имя) | `text-4xl sm:text-5xl md:text-6xl` | 700 |
| Подзаголовок | `text-xl sm:text-2xl` | 400 |
| Текст | `text-base sm:text-lg` | 400 |
| Контактная ссылка | `text-lg` | 600 |
| Footer | `text-sm` | 400 |

### Line Height
- Заголовок: `leading-tight`
- Текст: `leading-relaxed`

## Color Palette

### Light Theme (default)
| Token | Tailwind | Hex |
|-------|----------|-----|
| Background | `bg-white` | #ffffff |
| Text primary | `text-gray-900` | #111827 |
| Text secondary | `text-gray-600` | #4b5563 |
| Accent (ссылки) | `text-blue-600` | #2563eb |
| Accent hover | `text-blue-700` | #1d4ed8 |
| Footer text | `text-gray-400` | #9ca3af |
| Divider | `border-gray-200` | #e5e7eb |

## Spacing

| Отношение | Значение |
|-----------|----------|
| Между секциями | `gap-8 md:gap-12` |
| Между заголовком и подзаголовком | `gap-3` |
| Между подзаголовком и контактом | `gap-6` |
| Footer отступ | `mt-auto py-6` |

## Interaction

- Контактная ссылка (email) — стандартный `<a href="mailto:...">` с hover-эффектом (подчёркивание)
- Без анимаций, без JavaScript
- Без навигации (одна страница)
- Без состояний загрузки (статическая страница)

## Responsive Breakpoints

| Breakpoint | Изменения |
|-----------|-----------|
| Default (320px+) | Базовые стили, `text-base` |
| `sm` (640px+) | Увеличенный padding, `text-lg` |
| `md` (768px+) | Увеличенные заголовки, `text-2xl` |
| `lg` (1024px+) | Максимальный размер заголовка, большие отступы |

## Copywriting

### Тексты (плейсхолдеры)
| Ключ | Текст |
|------|-------|
| `title` | [Имя Фамилия] |
| `tagline` | Сайт скоро откроется |
| `contact_label` | Написать на почту |
| `contact_email` | [email@example.com] |
| `footer` | © 2026 [Имя Фамилия] |

**Примечание:** все тексты в квадратных скобках заменяются на реальные данные перед деплоем.

## Design System

### Решения
- Без компонентной библиотеки — чистый Tailwind
- Без CSS-переменных на фазе 1 (добавить в фазе 3 для тёмной темы)
- Без иконок на фазе 1 (добавить Lucide в фазе 2 для соцсетей)
- Без изображений (фото появится в фазе 2)
