---
name: frontend-design
description: Aesthetic direction — mood, density, visual device, anti-template patterns. Read BEFORE the first screen.
user-invocable: false
---

# Skill: Frontend Design
Version: 1.0 | Layer: Aesthetic Direction — read BEFORE the first screen

## Why this skill
Most AI-generated interfaces look the same:
blue primary, Inter, cards in a grid, rounded-lg everywhere.
This skill forces a conscious design decision
BEFORE writing code — and sticking to it.

---

## Step 1 — Choose an Aesthetic Direction

Before writing the first component — answer three questions:

**1. What is the product's mood?**
- Professional / business → strict typography, restrained palette
- Friendly / consumer → rounded shapes, warm colors
- Technical / tool-like → monochrome, dense information, mono fonts
- Creative / editorial → non-standard layout, bold fonts
- Premium / luxury → lots of space, thin lines, serif

**2. Information density?**
- Sparse (lots of air) → SaaS landing, portfolio, marketing
- Balanced → most web applications
- Dense (maximum data) → trading, analytics, devtools

**3. Main visual device?**
Choose ONE — this is the anchor of the entire design:
- Typography as hero (huge headings, bold weights)
- Color as hero (one strong accent color, everything else neutral)
- Space as hero (radical whitespace, minimalism)
- Geometry as hero (grid, lines, structure)
- Texture as hero (glassmorphism, grain, noise — carefully)

---

## Step 2 — Forbidden combinations (anti-template)

```
❌ Inter + #3b82f6 + white background + rounded-lg everywhere
❌ Gradient hero section with "Get Started" button in the center
❌ Three column features with icons on top
❌ "Testimonials" section with avatars in circles
❌ Footer with 4 columns of links
❌ Navbar: logo left, links center, CTA right (without a reason)
```

If you catch yourself doing one of these — stop and rethink.

---

## Step 3 — Concrete decisions before the first line of code

Lock these decisions in `design-system/tokens.ts` and `design-system/theme.css`:

### Color
```
Primary:  [specific hex — NOT default tailwind]
Neutral:  [warm / cool / pure gray]
Accent:   [one contrasting color or none at all]
Mode:     [light only / dark only / both]
```

### Typography
```
Display font: [specific — not Inter]
Body font:    [specific]
Mono font:    [if needed]
Scale:        [what size h1 — 36px? 48px? 60px?]
```

### Space
```
Base unit:    [4px or 6px or 8px]
Page padding: [how much edge padding]
Card padding: [internal card padding]
```

### Shape
```
Border radius: [sharp / soft / pill]
Borders:       [yes / no / dividers only]
Shadows:       [flat / subtle / elevated]
```

---

## Step 4 — References by project type

### SaaS / Productivity
Look at: Linear, Vercel, Raycast, Loom
Techniques: dark theme as primary, mono fonts for data,
thin borders instead of shadows, kbd shortcuts always visible

### Marketplace / E-commerce
Look at: Airbnb, Are.na, Gumroad
Techniques: photos as the main element, minimal chrome around content,
large cards, price always prominent

### Dashboard / Analytics
Look at: Grafana (new), Retool, Metabase
Techniques: dense grid, tabular nums everywhere, color only for data,
sidebar navigation, many data states

### CRM / B2B
Look at: Attio, Twenty, Notion
Techniques: tables as first-class UI, inline editing,
keyboard commands, neutral palette

### Consumer App
Look at: Arc, Fey, Craft
Techniques: animations as part of UX, personality in details,
custom icons, delight moments

---

## Step 5 — Typography as first impression

The fastest decision that changes everything — the main page heading.

```tsx
// BAD — template:
<h1 className="text-4xl font-bold text-gray-900">
  The platform for modern teams
</h1>

// GOOD — decision made:
// Choice: Fraunces (variable, optical sizing) — editorial, with character
// Size: clamp(48px, 8vw, 80px) — responsive
// Weight: 900 — maximum expressiveness
// Color: near-black with warmth, not #000000

<h1 className="font-display text-[clamp(3rem,8vw,5rem)]
               font-black tracking-tighter leading-none
               text-neutral-950 dark:text-neutral-50">
  From idea to<br/>
  <span className="text-primary-500">shipped product</span>
</h1>
```

---

## Step 6 — Component decisions

### Buttons — not just rounded
```tsx
// Choose ONE button style for the entire project:

// Style A: Flat + border (minimal, professional)
className="border border-neutral-900 bg-transparent px-5 py-2.5
           text-sm font-medium hover:bg-neutral-900 hover:text-white
           transition-colors"

// Style B: Filled + no border (confident, modern)
className="bg-primary-600 text-white px-5 py-2.5 text-sm font-medium
           hover:bg-primary-700 active:scale-[0.98] transition-all"

// Style C: Pill (friendly, consumer)
className="rounded-full bg-primary-600 text-white px-6 py-2.5
           text-sm font-medium shadow-sm hover:shadow-md transition-all"

// Style D: Brutal (bold, editorial)
className="bg-neutral-900 text-white px-5 py-2.5 text-sm font-semibold
           border-2 border-neutral-900 hover:bg-transparent
           hover:text-neutral-900 transition-colors"
```

### Cards — decide once
```tsx
// Flat (no shadows, border only):
className="rounded-lg border border-neutral-200 dark:border-neutral-800 p-5"

// Elevated (shadows instead of borders):
className="rounded-xl shadow-md hover:shadow-lg transition-shadow p-5 bg-white"

// Glass (on dark background):
className="rounded-xl border border-white/10 bg-white/5 backdrop-blur-sm p-5"

// Tinted (colored background):
className="rounded-lg bg-primary-50 dark:bg-primary-950/30 p-5"
```

---

## Checklist before first PR

```
[ ] Aesthetic direction locked (document or comment in tokens.ts)
[ ] Font chosen — not Inter as the only one
[ ] Primary color — not default tailwind blue
[ ] Button style chosen — one for the entire project
[ ] Card style chosen — one for the entire project
[ ] Reference product identified ("we're like X but...")
[ ] Dark mode: decided whether needed from day one
[ ] Main visual device — no more than one
```

## Known pitfalls
- Two "main visual devices" conflict and kill both
- Changing aesthetic direction after 30% development — expensive
- "Minimalism" without a system — is not minimalism, it's unfinished
- Custom fonts without `font-display: swap` block rendering

## Evolution log
- v1.0: initial template from bootstrap
