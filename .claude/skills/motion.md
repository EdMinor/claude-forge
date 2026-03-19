# Skill: Motion & Micro-interactions
Version: 1.0 | Layer: Feel — делает интерфейс живым

## Зачем этот скилл
Статичный интерфейс — это PDF. Движение передаёт смысл:
появление = "вот новое", уход = "ушло", пружина = "отклик".
Цель — не украшение, а коммуникация через физику.

## Библиотека
```bash
npm install framer-motion
```

---

## Паттерн 1 — Page Transitions (между роутами)

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

// В роутере — обёртка AnimatePresence:
import { AnimatePresence } from 'framer-motion'
import { useLocation } from 'react-router-dom'

export function AnimatedRoutes() {
  const location = useLocation()
  return (
    <AnimatePresence mode="wait">
      <Routes location={location} key={location.pathname}>
        {/* маршруты */}
      </Routes>
    </AnimatePresence>
  )
}
```

---

## Паттерн 2 — List Animations (stagger)

```tsx
// Элементы списка появляются с задержкой — создаёт ощущение "живости"
import { motion } from 'framer-motion'

const containerVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.06,  // задержка между элементами
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

## Паттерн 3 — Hover & Tap States

```tsx
// Карточки с физическим откликом:
<motion.div
  whileHover={{ y: -2, scale: 1.01 }}
  whileTap={{ scale: 0.98 }}
  transition={{ type: 'spring', stiffness: 400, damping: 25 }}
  className="cursor-pointer rounded-lg border..."
>
  {/* контент */}
</motion.div>

// Кнопки — spring bounce при нажатии:
<motion.button
  whileHover={{ scale: 1.02 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: 'spring', stiffness: 500, damping: 30 }}
>
  Сохранить
</motion.button>

// Иконки — rotate при hover:
<motion.div whileHover={{ rotate: 15 }} transition={{ type: 'spring' }}>
  <SettingsIcon />
</motion.div>
```

---

## Паттерн 4 — Modal & Sheet Animations

```tsx
// Модальное окно — fade + scale:
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

// Sheet снизу — slide up:
const sheetVariants = {
  hidden:  { y: '100%' },
  visible: { y: 0, transition: { type: 'spring', stiffness: 400, damping: 40 } },
  exit:    { y: '100%', transition: { duration: 0.2 } },
}
```

---

## Паттерн 5 — Number/Counter Animations

```tsx
// Анимированные числа в дашборде:
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

// Использование:
<AnimatedNumber value={12_450} />  // → анимирует от 0 до 12,450
```

---

## Паттерн 6 — Layout Animations (автоматические)

```tsx
// framer-motion автоматически анимирует изменения layout:
// Просто добавь layout prop — и перемещение элементов станет плавным

<motion.div layout className="grid grid-cols-3 gap-4">
  {items.map(item => (
    <motion.div key={item.id} layout>
      <Card item={item} />
    </motion.div>
  ))}
</motion.div>

// При добавлении/удалении элементов — AnimatePresence:
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

## Правила хорошей анимации

```
ДЕЛАЙ:
✓ duration 150–350ms для UI элементов
✓ spring для физических объектов (карточки, кнопки)
✓ tween/ease для фоновых элементов (overlay, page)
✓ stagger для списков — создаёт нарратив
✓ reducedMotion — всегда!

НЕ ДЕЛАЙ:
✗ duration > 500ms для элементов управления
✗ bounce на системных диалогах — раздражает
✗ анимировать width/height напрямую — используй scale
✗ анимировать layout без AnimatePresence на exit
```

### Обязательно: reduced motion

```tsx
// src/hooks/useReducedMotion.ts
import { useReducedMotion as useFramerReducedMotion } from 'framer-motion'

// В компонентах:
const shouldReduceMotion = useReducedMotion()
const variants = shouldReduceMotion
  ? { hidden: { opacity: 0 }, show: { opacity: 1 } }  // только fade
  : fullAnimationVariants

// Глобально в CSS:
// @media (prefers-reduced-motion: reduce) {
//   * { animation-duration: 0.01ms !important; }
// }
```

---

## Чеклист

```
[ ] Page transitions работают при навигации
[ ] Списки используют stagger при первом рендере
[ ] Карточки имеют hover state (хотя бы y: -2)
[ ] Кнопки имеют whileTap scale
[ ] Модальные окна анимированы (не просто appear)
[ ] reducedMotion учтён
[ ] Числа в KPI-карточках анимированы
```

## Known pitfalls
- AnimatePresence должен быть СНАРУЖИ от условного рендера
- `layout` prop конфликтует с некоторыми CSS Grid настройками
- spring анимации не имеют фиксированной duration — нельзя синхронизировать с CSS

## Evolution log
- v1.0: базовый шаблон из bootstrap
