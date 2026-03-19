# Skill: Typography
Version: 1.0 | Layer: Identity — второе что видит глаз после цвета

## Зачем этот скилл
Inter на 16px — это невидимость. Типографика создаёт личность,
иерархию и читаемость. Правильная пара шрифтов делает проект
"дорогим" без единого изменения в компонентах.

---

## Шаг 1 — Font Pairing

Выбери ОДНУ пару и держись её. Стратегии:

### Вариант A: Contrast Pair (контраст характеров)
Display serif + Clean sans — классика editorial
```
Display: Fraunces, Playfair Display, DM Serif Display
Body:    DM Sans, Outfit, Plus Jakarta Sans
```

### Вариант B: Family Pair (одно семейство)
Согласованность, modern SaaS feel
```
Display: Geist (bold/black weights)
Body:    Geist (regular/medium)
Mono:    Geist Mono
```

### Вариант C: Geometric Pair (чёткость + уют)
```
Display: Space Grotesk / Syne
Body:    Inter (ТОЛЬКО если Display выразительный)
```

### Подключение через fontsource (рекомендую вместо Google Fonts)
```bash
npm install @fontsource-variable/plus-jakarta-sans
npm install @fontsource-variable/dm-sans
npm install @fontsource/jetbrains-mono
```

```ts
// src/main.tsx
import '@fontsource-variable/plus-jakarta-sans'
import '@fontsource-variable/dm-sans'
import '@fontsource/jetbrains-mono/400.css'
import '@fontsource/jetbrains-mono/500.css'
```

---

## Шаг 2 — Hierarchy (не просто размер)

```
НЕ ТАК (только размер):
h1 = 32px, h2 = 24px, h3 = 18px, body = 16px

ТАК (размер + вес + трекинг + цвет):
h1 = 36px, weight 700, tracking -0.025em, color neutral-900
h2 = 24px, weight 600, tracking -0.015em, color neutral-800
h3 = 18px, weight 600, tracking 0,        color neutral-800
h4 = 15px, weight 600, tracking 0.01em,   color neutral-700  ← UPPERCASE label
body = 15px, weight 400, leading 1.6,     color neutral-700
small = 13px, weight 400, leading 1.5,    color neutral-500
```

### Tailwind классы для системы:
```tsx
// src/components/ui/typography.tsx
export const typographyVariants = {
  h1: 'font-display text-4xl font-bold tracking-tight text-neutral-900 dark:text-neutral-50',
  h2: 'font-display text-2xl font-semibold tracking-tight text-neutral-800 dark:text-neutral-100',
  h3: 'text-lg font-semibold text-neutral-800 dark:text-neutral-200',
  h4: 'text-sm font-semibold uppercase tracking-wider text-neutral-500 dark:text-neutral-400',
  body: 'text-[15px] leading-relaxed text-neutral-700 dark:text-neutral-300',
  small: 'text-[13px] text-neutral-500 dark:text-neutral-400',
  label: 'text-xs font-medium uppercase tracking-wider text-neutral-400',
  mono: 'font-mono text-[13px]',
}
```

---

## Шаг 3 — Детали которые делают разницу

### Числа: tabular lining (выравниваются в колонках)
```css
.tabular { font-variant-numeric: tabular-nums; }
/* Для таблиц, цен, статистики — всегда! */
```

### Заголовки: оптический kerning
```css
h1, h2 { text-rendering: optimizeLegibility; }
```

### Длинный текст: ограничь ширину
```css
/* Читаемость ухудшается > 70 символов в строке */
.prose { max-width: 65ch; }
```

### Ссылки: не подчёркивание по умолчанию
```css
a {
  text-decoration: none;
  color: var(--color-primary-600);
  border-bottom: 1px solid transparent;
  transition: border-color 0.15s;
}
a:hover { border-bottom-color: var(--color-primary-600); }
```

### Uppercase labels — только маленькие, с трекингом
```tsx
// Правильно: маленький размер + tracking-wide = читаемо
<span className="text-xs font-medium uppercase tracking-wider text-neutral-400">
  Статус клиента
</span>

// Неправильно: большой uppercase без трекинга — кричит
```

---

## Шаг 4 — Паттерны для типичных ситуаций

### Page Header (с подзаголовком)
```tsx
<div className="mb-8">
  <h1 className="font-display text-3xl font-bold tracking-tight text-neutral-900 dark:text-white">
    {title}
  </h1>
  {subtitle && (
    <p className="mt-1.5 text-[15px] text-neutral-500 dark:text-neutral-400">
      {subtitle}
    </p>
  )}
</div>
```

### Card Header (compact)
```tsx
<div className="mb-4">
  <h3 className="text-sm font-semibold text-neutral-900 dark:text-neutral-100">
    {title}
  </h3>
  <p className="text-xs text-neutral-500 mt-0.5">{description}</p>
</div>
```

### Статистика (большое число + лейбл)
```tsx
<div>
  <p className="text-3xl font-bold tabular-nums tracking-tight text-neutral-900">
    {value}
  </p>
  <p className="mt-1 text-xs font-medium uppercase tracking-wider text-neutral-400">
    {label}
  </p>
</div>
```

### Badge / Tag
```tsx
// НЕ: <span className="badge">Active</span>
// ДА: контекстный цвет + маленький текст + pill
<span className="inline-flex items-center rounded-full px-2.5 py-0.5
                 bg-emerald-50 text-emerald-700 dark:bg-emerald-900/30 dark:text-emerald-400
                 text-xs font-medium">
  Активен
</span>
```

---

## Чеклист

```
[ ] Два шрифта подключены и применены
[ ] h4 / labels используют uppercase + tracking, не просто bold
[ ] Таблицы с числами используют tabular-nums
[ ] prose контент ограничен max-width: 65ch
[ ] Dark mode для всех типографических цветов
[ ] Нет raw color: #333 или color: black в стилях
```

## Known pitfalls
- Variable fonts (Plus Jakarta Sans Variable) не работают как weight 700 в старых Safari
- `tracking-tight` на маленьких размерах (< 14px) ухудшает читаемость
- `font-display: swap` — добавляй всегда чтобы не было FOUT

## Evolution log
- v1.0: базовый шаблон из bootstrap
