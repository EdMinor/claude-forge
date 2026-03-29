---
name: typography
description: Identity layer — font pairing, text hierarchy, typographic details. Read after design-tokens, before first component.
user-invocable: false
---

# Skill: Typography
Version: 1.0 | Layer: Identity — the second thing the eye sees after color

## Why this skill
Inter at 16px is invisibility. Typography creates personality,
hierarchy, and readability. The right font pair makes a project
look "premium" without changing a single component.

---

## Step 1 — Font Pairing

Choose ONE pair and stick with it. Strategies:

### Option A: Contrast Pair (contrasting characters)
Display serif + Clean sans — classic editorial
```
Display: Fraunces, Playfair Display, DM Serif Display
Body:    DM Sans, Outfit, Plus Jakarta Sans
```

### Option B: Family Pair (same family)
Consistency, modern SaaS feel
```
Display: Geist (bold/black weights)
Body:    Geist (regular/medium)
Mono:    Geist Mono
```

### Option C: Geometric Pair (clarity + warmth)
```
Display: Space Grotesk / Syne
Body:    Inter (ONLY if Display is expressive)
```

### Installation via fontsource (recommended over Google Fonts)
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

## Step 2 — Hierarchy (not just size)

```
DON'T (size only):
h1 = 32px, h2 = 24px, h3 = 18px, body = 16px

DO (size + weight + tracking + color):
h1 = 36px, weight 700, tracking -0.025em, color neutral-900
h2 = 24px, weight 600, tracking -0.015em, color neutral-800
h3 = 18px, weight 600, tracking 0,        color neutral-800
h4 = 15px, weight 600, tracking 0.01em,   color neutral-700  ← UPPERCASE label
body = 15px, weight 400, leading 1.6,     color neutral-700
small = 13px, weight 400, leading 1.5,    color neutral-500
```

### Tailwind classes for the system:
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

## Step 3 — Details that make the difference

### Numbers: tabular lining (align in columns)
```css
.tabular { font-variant-numeric: tabular-nums; }
/* For tables, prices, statistics — always! */
```

### Headings: optical kerning
```css
h1, h2 { text-rendering: optimizeLegibility; }
```

### Long text: limit width
```css
/* Readability degrades beyond ~70 characters per line */
.prose { max-width: 65ch; }
```

### Links: no default underline
```css
a {
  text-decoration: none;
  color: var(--color-primary-600);
  border-bottom: 1px solid transparent;
  transition: border-color 0.15s;
}
a:hover { border-bottom-color: var(--color-primary-600); }
```

### Uppercase labels — only small, with tracking
```tsx
// Correct: small size + tracking-wide = readable
<span className="text-xs font-medium uppercase tracking-wider text-neutral-400">
  Client Status
</span>

// Wrong: large uppercase without tracking — screams
```

---

## Step 4 — Patterns for common situations

### Page Header (with subtitle)
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

### Statistic (large number + label)
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
// DON'T: <span className="badge">Active</span>
// DO: contextual color + small text + pill
<span className="inline-flex items-center rounded-full px-2.5 py-0.5
                 bg-emerald-50 text-emerald-700 dark:bg-emerald-900/30 dark:text-emerald-400
                 text-xs font-medium">
  Active
</span>
```

---

## Checklist

```
[ ] Two fonts connected and applied
[ ] h4 / labels use uppercase + tracking, not just bold
[ ] Tables with numbers use tabular-nums
[ ] Prose content limited to max-width: 65ch
[ ] Dark mode for all typographic colors
[ ] No raw color: #333 or color: black in styles
```

## Known pitfalls
- Variable fonts (Plus Jakarta Sans Variable) don't work as weight 700 in older Safari
- `tracking-tight` on small sizes (< 14px) hurts readability
- `font-display: swap` — always add it to prevent FOUT

## Evolution log
- v1.0: initial template from bootstrap
