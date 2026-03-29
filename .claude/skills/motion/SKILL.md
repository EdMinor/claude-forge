---
name: motion
description: Feel layer — Framer Motion patterns for page transitions, list stagger, hover/tap states, modals, and number animations. Apply after base structure.
user-invocable: false
---

# Skill: Motion & Micro-interactions
Version: 1.0 | Layer: Feel — makes the interface alive

## Why this skill
A static interface is a PDF. Motion conveys meaning:
appearing = "here's something new", leaving = "it's gone", spring = "response".
The goal is not decoration, but communication through physics.

## Library
```bash
npm install framer-motion
```

---

## Pattern 1 — Page Transitions (between routes)

```tsx
// src/components/layout/PageTransition.tsx
import { motion } from 'framer-motion'

const pageVariants = {
  initial: { opacity: 0, y: 8 },
  in:      { opacity: 1, y: 0 },
  out:     { opacity: 0, y: -8 },
}

const pageTransition = {
  type: 'tween',
  ease: 'anticipate',
  duration: 0.25,
}

export function PageTransition({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial="initial"
      animate="in"
      exit="out"
      variants={pageVariants}
      transition={pageTransition}
    >
      {children}
    </motion.div>
  )
}

// In the router — wrap with AnimatePresence:
import { AnimatePresence } from 'framer-motion'
import { useLocation } from 'react-router-dom'

export function AnimatedRoutes() {
  const location = useLocation()
  return (
    <AnimatePresence mode="wait">
      <Routes location={location} key={location.pathname}>
        {/* routes */}
      </Routes>
    </AnimatePresence>
  )
}
```

---

## Pattern 2 — List Animations (stagger)

```tsx
// List items appear with a delay — creates a sense of "liveliness"
import { motion } from 'framer-motion'

const containerVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.06,  // delay between items
    },
  },
}

const itemVariants = {
  hidden: { opacity: 0, y: 12 },
  show:   { opacity: 1, y: 0, transition: { type: 'spring', stiffness: 400, damping: 30 } },
}

export function AnimatedList({ items }: { items: any[] }) {
  return (
    <motion.ul
      variants={containerVariants}
      initial="hidden"
      animate="show"
    >
      {items.map((item) => (
        <motion.li key={item.id} variants={itemVariants}>
          <ItemCard item={item} />
        </motion.li>
      ))}
    </motion.ul>
  )
}
```

---

## Pattern 3 — Hover & Tap States

```tsx
// Cards with physical feedback:
<motion.div
  whileHover={{ y: -2, scale: 1.01 }}
  whileTap={{ scale: 0.98 }}
  transition={{ type: 'spring', stiffness: 400, damping: 25 }}
  className="cursor-pointer rounded-lg border..."
>
  {/* content */}
</motion.div>

// Buttons — spring bounce on press:
<motion.button
  whileHover={{ scale: 1.02 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: 'spring', stiffness: 500, damping: 30 }}
>
  Save
</motion.button>

// Icons — rotate on hover:
<motion.div whileHover={{ rotate: 15 }} transition={{ type: 'spring' }}>
  <SettingsIcon />
</motion.div>
```

---

## Pattern 4 — Modal & Sheet Animations

```tsx
// Modal — fade + scale:
const modalVariants = {
  hidden:  { opacity: 0, scale: 0.95, y: 10 },
  visible: { opacity: 1, scale: 1,    y: 0 },
  exit:    { opacity: 0, scale: 0.95, y: 10 },
}

// Overlay — fade:
const overlayVariants = {
  hidden:  { opacity: 0 },
  visible: { opacity: 1 },
  exit:    { opacity: 0 },
}

// Bottom sheet — slide up:
const sheetVariants = {
  hidden:  { y: '100%' },
  visible: { y: 0, transition: { type: 'spring', stiffness: 400, damping: 40 } },
  exit:    { y: '100%', transition: { duration: 0.2 } },
}
```

---

## Pattern 5 — Number/Counter Animations

```tsx
// Animated numbers in dashboard:
import { useMotionValue, useSpring, useTransform, animate } from 'framer-motion'
import { useEffect } from 'react'

function AnimatedNumber({ value }: { value: number }) {
  const count = useMotionValue(0)
  const rounded = useTransform(count, (v) => Math.round(v).toLocaleString())

  useEffect(() => {
    const controls = animate(count, value, { duration: 1.2, ease: 'easeOut' })
    return controls.stop
  }, [value])

  return <motion.span>{rounded}</motion.span>
}

// Usage:
<AnimatedNumber value={12_450} />  // → animates from 0 to 12,450
```

---

## Pattern 6 — Layout Animations (automatic)

```tsx
// framer-motion automatically animates layout changes:
// Just add the layout prop — and element movement becomes smooth

<motion.div layout className="grid grid-cols-3 gap-4">
  {items.map(item => (
    <motion.div key={item.id} layout>
      <Card item={item} />
    </motion.div>
  ))}
</motion.div>

// When adding/removing elements — AnimatePresence:
<AnimatePresence>
  {items.map(item => (
    <motion.div
      key={item.id}
      layout
      initial={{ opacity: 0, scale: 0.8 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.8 }}
    />
  ))}
</AnimatePresence>
```

---

## Rules for good animation

```
DO:
✓ duration 150–350ms for UI elements
✓ spring for physical objects (cards, buttons)
✓ tween/ease for background elements (overlay, page)
✓ stagger for lists — creates narrative
✓ reducedMotion — always!

DON'T:
✗ duration > 500ms for controls
✗ bounce on system dialogs — annoying
✗ animate width/height directly — use scale
✗ animate layout without AnimatePresence on exit
```

### Required: reduced motion

```tsx
// src/hooks/useReducedMotion.ts
import { useReducedMotion as useFramerReducedMotion } from 'framer-motion'

// In components:
const shouldReduceMotion = useReducedMotion()
const variants = shouldReduceMotion
  ? { hidden: { opacity: 0 }, show: { opacity: 1 } }  // fade only
  : fullAnimationVariants

// Globally in CSS:
// @media (prefers-reduced-motion: reduce) {
//   * { animation-duration: 0.01ms !important; }
// }
```

---

## Checklist

```
[ ] Page transitions work during navigation
[ ] Lists use stagger on first render
[ ] Cards have hover state (at least y: -2)
[ ] Buttons have whileTap scale
[ ] Modals are animated (not just appear)
[ ] reducedMotion respected
[ ] Numbers in KPI cards are animated
```

## Known pitfalls
- AnimatePresence must be OUTSIDE of conditional rendering
- `layout` prop conflicts with some CSS Grid settings
- spring animations don't have a fixed duration — can't sync with CSS

## Evolution log
- v1.0: initial template from bootstrap
