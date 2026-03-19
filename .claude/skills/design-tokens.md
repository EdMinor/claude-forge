# Skill: Design Tokens
Version: 1.0 | Layer: Foundation — читать ПЕРВЫМ, до любого UI

## Зачем этот скилл
Дефолтный синий shadcn `#3b82f6` и шрифт Inter — это визуальная подпись
"сгенерировано AI". Токены — это первое что нужно переопределить.
Делается один раз, влияет на весь проект.

---

## Шаг 1 — Выбери палитру с характером

НЕ делай так (generic):
```ts
primary: '#3b82f6'  // дефолтный tailwind blue — стоп
```

ДЕЛАЙ так — выбери нестандартный якорный цвет:
```ts
// Примеры нешаблонных якорей:
// Slate blue:  #6366f1  →  indigo с характером
// Forest:      #16a34a  →  зелёный не-по-умолчанию
// Ember:       #ea580c  →  оранжево-красный, энергичный
// Ink:         #1e293b  →  почти-чёрный с теплотой
// Violet:      #7c3aed  →  насыщенный фиолетовый
// Slate teal:  #0f766e  →  зелёно-серый, элегантный
```

Из якоря строй шкалу через CSS переменные:
```css
/* src/design-system/theme.css */
:root {
  /* Primary — выбери ОДИН якорный цвет, не дефолтный */
  --color-primary-50:  /* lightest tint */;
  --color-primary-100: ;
  --color-primary-200: ;
  --color-primary-500: /* якорь */;
  --color-primary-600: /* hover */;
  --color-primary-700: /* active */;
  --color-primary-900: /* darkest */;

  /* Neutral — уйди от серого, добавь температуру */
  /* Warm neutral (бежевый подтон): */
  --color-neutral-50:  #fafaf9;
  --color-neutral-100: #f5f5f4;
  --color-neutral-200: #e7e5e4;
  --color-neutral-400: #a8a29e;
  --color-neutral-600: #57534e;
  --color-neutral-800: #292524;
  --color-neutral-900: #1c1917;

  /* Surface */
  --surface-base:    var(--color-neutral-50);
  --surface-raised:  #ffffff;
  --surface-sunken:  var(--color-neutral-100);

  /* Semantic */
  --color-success: #16a34a;
  --color-warning: #d97706;
  --color-error:   #dc2626;
  --color-info:    var(--color-primary-500);
}

/* Dark mode — НЕ просто инвертируй, переосмысли */
@media (prefers-color-scheme: dark) {
  :root {
    --surface-base:   #0f172a;  /* slate, не просто #000 */
    --surface-raised: #1e293b;
    --surface-sunken: #0a0f1e;
  }
}
```

---

## Шаг 2 — Type Scale (не дефолтный Tailwind)

```ts
// src/design-system/tokens.ts
export const typography = {
  // Шрифты — НЕ Inter, НЕ system-ui как единственный вариант
  // Варианты с характером:
  // Display: 'Fraunces', 'Playfair Display', 'DM Serif Display'
  // Sans:    'DM Sans', 'Plus Jakarta Sans', 'Outfit', 'Geist'
  // Mono:    'JetBrains Mono', 'Fira Code', 'Geist Mono'

  fontDisplay: '"Plus Jakarta Sans", sans-serif',  // заголовки
  fontBody:    '"DM Sans", sans-serif',             // текст
  fontMono:    '"JetBrains Mono", monospace',       // код

  // Размеры — используй нестандартный шаг
  size: {
    xs:   '0.75rem',   // 12px
    sm:   '0.875rem',  // 14px
    base: '1rem',      // 16px
    lg:   '1.125rem',  // 18px
    xl:   '1.25rem',   // 20px
    '2xl': '1.5rem',   // 24px
    '3xl': '1.875rem', // 30px
    '4xl': '2.25rem',  // 36px
    '5xl': '3rem',     // 48px
  },

  // Letter spacing — недооценённый инструмент
  tracking: {
    tight:  '-0.025em',  // большие заголовки
    normal: '0em',
    wide:   '0.025em',   // small caps, labels
    wider:  '0.05em',    // uppercase labels
    widest: '0.1em',     // decorative
  },

  // Line height — влияет на "воздух"
  leading: {
    none:    '1',
    tight:   '1.25',
    snug:    '1.375',
    normal:  '1.5',
    relaxed: '1.625',
    loose:   '2',
  },
}
```

---

## Шаг 3 — Spacing Scale (не стандартный Tailwind)

```ts
export const spacing = {
  // Базовая единица — попробуй 6px вместо 4px
  // Это сделает интерфейс "просторнее" автоматически
  base: 6,  // px

  scale: {
    0:    '0px',
    0.5:  '3px',
    1:    '6px',
    1.5:  '9px',
    2:    '12px',
    3:    '18px',
    4:    '24px',   // стандартный padding карточки
    5:    '30px',
    6:    '36px',
    8:    '48px',
    10:   '60px',
    12:   '72px',
    16:   '96px',
    20:   '120px',
  },

  // Радиусы — выбери одну систему и держись её
  radius: {
    sm:   '6px',
    md:   '10px',   // кнопки, инпуты
    lg:   '14px',   // карточки
    xl:   '20px',   // модальные окна
    full: '9999px', // пилюли
  },
}
```

---

## Шаг 4 — Shadows (минималистичные, с температурой)

```ts
export const shadows = {
  // Тёплые тени (для светлой темы с тёплым нейтралом):
  sm:  '0 1px 2px rgba(28, 25, 23, 0.06)',
  md:  '0 4px 6px -1px rgba(28, 25, 23, 0.08), 0 2px 4px -2px rgba(28, 25, 23, 0.04)',
  lg:  '0 10px 15px -3px rgba(28, 25, 23, 0.08), 0 4px 6px -4px rgba(28, 25, 23, 0.04)',
  xl:  '0 20px 25px -5px rgba(28, 25, 23, 0.1), 0 8px 10px -6px rgba(28, 25, 23, 0.04)',

  // Inner shadow для inset элементов:
  inner: 'inset 0 2px 4px rgba(28, 25, 23, 0.06)',
}
```

---

## Шаг 5 — Интеграция в Tailwind

```ts
// tailwind.config.ts
import { typography, spacing, shadows } from './src/design-system/tokens'

export default {
  theme: {
    extend: {
      fontFamily: {
        display: [typography.fontDisplay],
        sans:    [typography.fontBody],
        mono:    [typography.fontMono],
      },
      letterSpacing: typography.tracking,
      lineHeight:    typography.leading,
      borderRadius:  spacing.radius,
      boxShadow:     shadows,
      // Цвета через CSS переменные:
      colors: {
        primary: {
          50:  'var(--color-primary-50)',
          500: 'var(--color-primary-500)',
          600: 'var(--color-primary-600)',
          // ...
        },
        neutral: {
          50:  'var(--color-neutral-50)',
          // ...
        },
        surface: {
          base:   'var(--surface-base)',
          raised: 'var(--surface-raised)',
          sunken: 'var(--surface-sunken)',
        },
      },
    },
  },
}
```

---

## Checklists — до первого компонента

```
[ ] Якорный цвет выбран (не дефолтный синий)
[ ] Шрифты подключены через Google Fonts / fontsource
[ ] theme.css создан с CSS переменными
[ ] tokens.ts создан и экспортирован
[ ] tailwind.config.ts обновлён
[ ] Dark mode переменные определены
[ ] Проверено: ни одного raw #3b82f6 или 'Inter' в коде
```

## Known pitfalls
- Google Fonts в продакшне — используй `@fontsource/[font]` пакеты
- Не забудь `font-display: swap` для производительности
- CSS переменные в Tailwind: обёртывай в `rgb()` если нужна opacity

## Evolution log
- v1.0: базовый шаблон из bootstrap
