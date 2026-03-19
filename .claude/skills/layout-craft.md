# Skill: Layout Craft
Version: 1.0 | Layer: Composition — структура которую чувствуют, не замечая

## Зачем этот скилл
Шаблонный layout: карточки в grid с одинаковыми отступами.
Продвинутый: визуальная иерархия, breathing room, focal point.
Правило: хороший layout не замечают — плохой раздражает.

---

## Принцип 1 — Visual Hierarchy через размер и вес

```
Первый взгляд попадает на:
1. Самое большое
2. Самое контрастное
3. Самое изолированное (с пространством вокруг)

Каждый экран должен иметь ОДИН focal point.
Если всё одинаково важно — ничто не важно.
```

### Пример — Dashboard Hero
```tsx
// ПЛОХО: четыре одинаковые карточки в ряд
<div className="grid grid-cols-4 gap-4">
  <StatCard label="Сделки" value="42" />
  <StatCard label="Клиенты" value="128" />
  <StatCard label="Выручка" value="1.2M" />
  <StatCard label="Конверсия" value="18%" />
</div>

// ХОРОШО: hero-метрика + вторичные
<div className="grid grid-cols-12 gap-4">
  {/* Главная метрика — занимает больше */}
  <div className="col-span-5">
    <HeroMetric label="Выручка за месяц" value="1.2M ₽" change="+12%" />
  </div>
  {/* Вторичные — компактнее */}
  <div className="col-span-7 grid grid-cols-3 gap-4">
    <StatCard label="Сделки" value="42" />
    <StatCard label="Клиенты" value="128" />
    <StatCard label="Конверсия" value="18%" />
  </div>
</div>
```

---

## Принцип 2 — Breathing Room (воздух)

```
Правило: если кажется что отступов достаточно — добавь ещё.
Большинство шаблонов имеют padding: 16px там где нужно 24-32px.
```

```tsx
// Базовые отступы для разных контекстов:

// Page padding:
<main className="px-6 py-8 md:px-10 md:py-10 lg:px-16 lg:py-12">

// Card internal padding:
<div className="p-5 md:p-6">  // не p-4, это тесно

// Section spacing (между блоками):
<div className="space-y-10">  // не space-y-4

// Between label и value в stat:
<div className="space-y-1.5">  // не space-y-1
```

---

## Принцип 3 — Grid Systems (не просто grid-cols-3)

```tsx
// Базовая 12-колоночная сетка адаптируется умно:

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
    <MainChart />  {/* главный контент — больше */}
  </div>
  <div className="col-span-12 lg:col-span-4 space-y-4">
    <ActivityFeed />  {/* боковая панель */}
    <QuickStats />
  </div>
</div>

// Masonry-like card grid (разные высоты):
<div className="columns-1 md:columns-2 lg:columns-3 gap-4">
  {items.map(item => (
    <div key={item.id} className="break-inside-avoid mb-4">
      <Card item={item} />
    </div>
  ))}
</div>
```

---

## Принцип 4 — Dividers и Separators

```tsx
// НЕ используй <hr> везде — это грубо

// Тонкая граница через border-b:
<div className="border-b border-neutral-200 dark:border-neutral-800 pb-4 mb-4">
  <SectionHeader />
</div>

// Пространство как разделитель (лучше бордера):
<div className="mt-8 pt-8">  {/* gap без видимой линии */}

// Divider с текстом:
<div className="relative my-6">
  <div className="absolute inset-0 flex items-center">
    <div className="w-full border-t border-neutral-200" />
  </div>
  <div className="relative flex justify-center">
    <span className="bg-white px-4 text-xs text-neutral-400 uppercase tracking-wider">
      или
    </span>
  </div>
</div>
```

---

## Принцип 5 — Sticky & Pinned Elements

```tsx
// Sticky header в таблицах:
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
  {/* backdrop-blur создаёт depth */}
</header>

// Floating action button:
<div className="fixed bottom-6 right-6 z-50">
  <button className="rounded-full shadow-lg ...">
    <PlusIcon />
  </button>
</div>
```

---

## Принцип 6 — Empty Space как Design Element

```tsx
// Большой h1 с щедрым padding сверху:
<div className="pt-16 pb-8">
  <h1>Добро пожаловать</h1>
</div>

// Centered content с max-width и auto margins:
<div className="mx-auto max-w-3xl">
  <ArticleContent />
</div>

// Вертикальный ритм через consistent spacing:
// Используй только значения из spacing scale: 4, 6, 8, 10, 12, 16
// Никогда: mt-5, pb-7, gap-3 (непоследовательно)
```

---

## Паттерн — Page Layout Template

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

## Чеклист

```
[ ] Каждый экран имеет один primary focal point
[ ] Page padding: минимум px-6 py-8
[ ] Card padding: минимум p-5
[ ] Sections разделены пространством, не только бордерами
[ ] Dashboard использует асимметричный grid (не 4 равных карточки)
[ ] Sticky header использует backdrop-blur
[ ] max-width задан для content-heavy страниц
[ ] Spacing использует только значения из scale (нет mt-5 или gap-3)
```

## Known pitfalls
- `columns-*` (masonry) не работает с framer-motion layout
- `backdrop-blur` требует `bg-white/80` иначе нет эффекта
- `sticky` не работает внутри `overflow: hidden` родителя

## Evolution log
- v1.0: базовый шаблон из bootstrap
