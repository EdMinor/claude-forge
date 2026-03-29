---
name: design-tokens
description: Foundation layer — custom color palette, type scale, spacing, shadows, and Tailwind integration. Read FIRST before any UI work.
user-invocable: false
---

# Skill: Design Tokens
Version: 1.0 | Layer: Foundation — read FIRST, before any UI

## Why this skill
Default shadcn blue `#3b82f6` and Inter font are the visual signature
of "AI-generated." Tokens are the first thing to override.
Done once, affects the entire project.

---

## Step 1 — Choose a palette with character

DON'T do this (generic):
```ts
primary: '#3b82f6'  // default tailwind blue — stop
```

DO this — choose a non-standard anchor color:
```ts
// Non-template anchor examples:
// Slate blue:  #6366f1  →  indigo with character
// Forest:      #16a34a  →  non-default green
// Ember:       #ea580c  →  orange-red, energetic
// Ink:         #1e293b  →  near-black with warmth
// Violet:      #7c3aed  →  rich purple
// Slate teal:  #0f766e  →  green-gray, elegant
```

Build a scale from the anchor using CSS variables:
```css
/* src/design-system/theme.css */
:root {
  /* Primary — choose ONE anchor color, not default */
  --color-primary-50:  /* lightest tint */;
  --color-primary-100: ;
  --color-primary-200: ;
  --color-primary-500: /* anchor */;
  --color-primary-600: /* hover */;
  --color-primary-700: /* active */;
  --color-primary-900: /* darkest */;

  /* Neutral — move away from gray, add temperature */
  /* Warm neutral (beige undertone): */
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

/* Dark mode — DON'T just invert, rethink */
@media (prefers-color-scheme: dark) {
  :root {
    --surface-base:   #0f172a;  /* slate, not just #000 */
    --surface-raised: #1e293b;
    --surface-sunken: #0a0f1e;
  }
}
```

---

## Step 2 — Type Scale (not default Tailwind)

```ts
// src/design-system/tokens.ts
export const typography = {
  // Fonts — NOT Inter, NOT system-ui as the only option
  // Options with character:
  // Display: 'Fraunces', 'Playfair Display', 'DM Serif Display'
  // Sans:    'DM Sans', 'Plus Jakarta Sans', 'Outfit', 'Geist'
  // Mono:    'JetBrains Mono', 'Fira Code', 'Geist Mono'

  fontDisplay: '"Plus Jakarta Sans", sans-serif',  // headings
  fontBody:    '"DM Sans", sans-serif',             // body text
  fontMono:    '"JetBrains Mono", monospace',       // code

  // Sizes — use a non-standard step
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

  // Letter spacing — an underrated tool
  tracking: {
    tight:  '-0.025em',  // large headings
    normal: '0em',
    wide:   '0.025em',   // small caps, labels
    wider:  '0.05em',    // uppercase labels
    widest: '0.1em',     // decorative
  },

  // Line height — affects "breathing room"
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

## Step 3 — Spacing Scale (not standard Tailwind)

```ts
export const spacing = {
  // Base unit — try 6px instead of 4px
  // This automatically makes the interface feel more "spacious"
  base: 6,  // px

  scale: {
    0:    '0px',
    0.5:  '3px',
    1:    '6px',
    1.5:  '9px',
    2:    '12px',
    3:    '18px',
    4:    '24px',   // standard card padding
    5:    '30px',
    6:    '36px',
    8:    '48px',
    10:   '60px',
    12:   '72px',
    16:   '96px',
    20:   '120px',
  },

  // Border radius — choose one system and stick with it
  radius: {
    sm:   '6px',
    md:   '10px',   // buttons, inputs
    lg:   '14px',   // cards
    xl:   '20px',   // modals
    full: '9999px', // pills
  },
}
```

---

## Step 4 — Shadows (minimalist, with temperature)

```ts
export const shadows = {
  // Warm shadows (for light theme with warm neutral):
  sm:  '0 1px 2px rgba(28, 25, 23, 0.06)',
  md:  '0 4px 6px -1px rgba(28, 25, 23, 0.08), 0 2px 4px -2px rgba(28, 25, 23, 0.04)',
  lg:  '0 10px 15px -3px rgba(28, 25, 23, 0.08), 0 4px 6px -4px rgba(28, 25, 23, 0.04)',
  xl:  '0 20px 25px -5px rgba(28, 25, 23, 0.1), 0 8px 10px -6px rgba(28, 25, 23, 0.04)',

  // Inner shadow for inset elements:
  inner: 'inset 0 2px 4px rgba(28, 25, 23, 0.06)',
}
```

---

## Step 5 — Tailwind Integration

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
      // Colors via CSS variables:
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

## Checklist — before first component

```
[ ] Anchor color chosen (not default blue)
[ ] Fonts connected via Google Fonts / fontsource
[ ] theme.css created with CSS variables
[ ] tokens.ts created and exported
[ ] tailwind.config.ts updated
[ ] Dark mode variables defined
[ ] Verified: no raw #3b82f6 or 'Inter' in code
```

## Known pitfalls
- Google Fonts in production — use `@fontsource/[font]` packages instead
- Don't forget `font-display: swap` for performance
- CSS variables in Tailwind: wrap in `rgb()` if you need opacity

## Evolution log
- v1.0: initial template from bootstrap
