---
name: layout-craft
description: Composition layer — visual hierarchy, breathing room, grid systems, sticky elements. Apply per screen.
user-invocable: false
---

# Skill: Layout Craft
Version: 1.0 | Layer: Composition — the structure people feel without noticing

## Why this skill
Template layout: cards in a grid with equal spacing.
Advanced layout: visual hierarchy, breathing room, focal point.
Rule: good layout goes unnoticed — bad layout irritates.

---

## Principle 1 — Visual Hierarchy through size and weight

```
The first glance lands on:
1. The biggest thing
2. The most contrasting thing
3. The most isolated thing (with space around it)

Every screen should have ONE focal point.
If everything is equally important — nothing is important.
```

### Example — Dashboard Hero
```tsx
// BAD: four identical cards in a row
<div className="grid grid-cols-4 gap-4">
  <StatCard label="Deals" value="42" />
  <StatCard label="Clients" value="128" />
  <StatCard label="Revenue" value="1.2M" />
  <StatCard label="Conversion" value="18%" />
</div>

// GOOD: hero metric + secondary ones
<div className="grid grid-cols-12 gap-4">
  {/* Main metric — takes more space */}
  <div className="col-span-5">
    <HeroMetric label="Monthly Revenue" value="$1.2M" change="+12%" />
  </div>
  {/* Secondary — more compact */}
  <div className="col-span-7 grid grid-cols-3 gap-4">
    <StatCard label="Deals" value="42" />
    <StatCard label="Clients" value="128" />
    <StatCard label="Conversion" value="18%" />
  </div>
</div>
```

---

## Principle 2 — Breathing Room (whitespace)

```
Rule: if the spacing seems enough — add more.
Most templates have padding: 16px where 24-32px is needed.
```

```tsx
// Base spacing for different contexts:

// Page padding:
<main className="px-6 py-8 md:px-10 md:py-10 lg:px-16 lg:py-12">

// Card internal padding:
<div className="p-5 md:p-6">  // not p-4, that's cramped

// Section spacing (between blocks):
<div className="space-y-10">  // not space-y-4

// Between label and value in stat:
<div className="space-y-1.5">  // not space-y-1
```

---

## Principle 3 — Grid Systems (not just grid-cols-3)

```tsx
// Basic 12-column grid adapts smartly:

// Sidebar layout:
<div className="grid grid-cols-12 gap-6">
  <aside className="col-span-12 md:col-span-3 lg:col-span-2">
    <Sidebar />
  </aside>
  <main className="col-span-12 md:col-span-9 lg:col-span-10">
    <Content />
  </main>
</div>

// Dashboard asymmetric layout:
<div className="grid grid-cols-12 gap-6">
  <div className="col-span-12 lg:col-span-8">
    <MainChart />  {/* main content — larger */}
  </div>
  <div className="col-span-12 lg:col-span-4 space-y-4">
    <ActivityFeed />  {/* side panel */}
    <QuickStats />
  </div>
</div>

// Masonry-like card grid (varying heights):
<div className="columns-1 md:columns-2 lg:columns-3 gap-4">
  {items.map(item => (
    <div key={item.id} className="break-inside-avoid mb-4">
      <Card item={item} />
    </div>
  ))}
</div>
```

---

## Principle 4 — Dividers and Separators

```tsx
// DON'T use <hr> everywhere — it's crude

// Thin border via border-b:
<div className="border-b border-neutral-200 dark:border-neutral-800 pb-4 mb-4">
  <SectionHeader />
</div>

// Space as a divider (better than a border):
<div className="mt-8 pt-8">  {/* gap without a visible line */}

// Divider with text:
<div className="relative my-6">
  <div className="absolute inset-0 flex items-center">
    <div className="w-full border-t border-neutral-200" />
  </div>
  <div className="relative flex justify-center">
    <span className="bg-white px-4 text-xs text-neutral-400 uppercase tracking-wider">
      or
    </span>
  </div>
</div>
```

---

## Principle 5 — Sticky & Pinned Elements

```tsx
// Sticky header in tables:
<div className="overflow-auto max-h-[600px]">
  <table>
    <thead className="sticky top-0 z-10 bg-white dark:bg-neutral-900
                      border-b border-neutral-200">
      <tr>...</tr>
    </thead>
    <tbody>...</tbody>
  </table>
</div>

// Sticky page header:
<header className="sticky top-0 z-40 border-b border-neutral-200
                   bg-white/80 dark:bg-neutral-900/80 backdrop-blur-sm">
  {/* backdrop-blur creates depth */}
</header>

// Floating action button:
<div className="fixed bottom-6 right-6 z-50">
  <button className="rounded-full shadow-lg ...">
    <PlusIcon />
  </button>
</div>
```

---

## Principle 6 — Empty Space as a Design Element

```tsx
// Large h1 with generous top padding:
<div className="pt-16 pb-8">
  <h1>Welcome</h1>
</div>

// Centered content with max-width and auto margins:
<div className="mx-auto max-w-3xl">
  <ArticleContent />
</div>

// Vertical rhythm through consistent spacing:
// Use only values from the spacing scale: 4, 6, 8, 10, 12, 16
// Never: mt-5, pb-7, gap-3 (inconsistent)
```

---

## Pattern — Page Layout Template

```tsx
// src/components/layout/PageLayout.tsx
interface PageLayoutProps {
  header: React.ReactNode
  children: React.ReactNode
  aside?: React.ReactNode
  maxWidth?: 'sm' | 'md' | 'lg' | 'xl' | 'full'
}

const maxWidths = {
  sm:   'max-w-2xl',
  md:   'max-w-4xl',
  lg:   'max-w-6xl',
  xl:   'max-w-7xl',
  full: 'max-w-none',
}

export function PageLayout({ header, children, aside, maxWidth = 'xl' }: PageLayoutProps) {
  return (
    <div className={`mx-auto ${maxWidths[maxWidth]} px-4 sm:px-6 lg:px-8`}>
      <div className="py-8 md:py-10">
        {header}
      </div>
      {aside ? (
        <div className="grid grid-cols-12 gap-6 pb-12">
          <main className="col-span-12 lg:col-span-8">{children}</main>
          <aside className="col-span-12 lg:col-span-4">{aside}</aside>
        </div>
      ) : (
        <div className="pb-12">{children}</div>
      )}
    </div>
  )
}
```

---

## Checklist

```
[ ] Every screen has one primary focal point
[ ] Page padding: at least px-6 py-8
[ ] Card padding: at least p-5
[ ] Sections separated by space, not just borders
[ ] Dashboard uses asymmetric grid (not 4 equal cards)
[ ] Sticky header uses backdrop-blur
[ ] max-width set for content-heavy pages
[ ] Spacing uses only values from the scale (no mt-5 or gap-3)
```

## Known pitfalls
- `columns-*` (masonry) doesn't work with framer-motion layout
- `backdrop-blur` requires `bg-white/80` otherwise no effect
- `sticky` doesn't work inside `overflow: hidden` parent

## Evolution log
- v1.0: initial template from bootstrap
