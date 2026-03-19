# Skill: Frontend Design
Version: 1.0 | Layer: Aesthetic Direction — читать ДО первого экрана

## Зачем этот скилл
Большинство AI-сгенерированных интерфейсов выглядят одинаково:
синий primary, Inter, карточки в grid, rounded-lg везде.
Этот скилл заставляет принять сознательное дизайн-решение
ДО написания кода — и держаться его.

---

## Шаг 1 — Выбери Aesthetic Direction

Прежде чем писать первый компонент — ответь на три вопроса:

**1. Какое настроение продукта?**
- Профессиональный / деловой → строгая типографика, сдержанная палитра
- Дружелюбный / потребительский → скруглённые формы, тёплые цвета
- Технический / инструментальный → монохром, плотная информация, mono-шрифты
- Творческий / editorial → нестандартный layout, смелые шрифты
- Премиальный / luxury → много пространства, тонкие линии, serif

**2. Плотность информации?**
- Sparse (много воздуха) → SaaS landing, portfolio, маркетинг
- Balanced → большинство веб-приложений
- Dense (максимум данных) → трейдинг, аналитика, devtools

**3. Главный визуальный приём?**
Выбери ОДИН — это якорь всего дизайна:
- Типографика как герой (огромные заголовки, смелые веса)
- Цвет как герой (один сильный акцентный цвет, всё остальное нейтральное)
- Пространство как герой (радикальный whitespace, минимализм)
- Геометрия как герой (сетка, линии, структура)
- Текстура как герой (glassmorphism, grain, noise — аккуратно)

---

## Шаг 2 — Запрещённые комбинации (антишаблон)

```
❌ Inter + #3b82f6 + white background + rounded-lg везде
❌ Gradient hero section с "Get Started" кнопкой посередине
❌ Три колонки features с иконками сверху
❌ "Testimonials" секция с аватарами в кружках
❌ Footer с 4 колонками ссылок
❌ Navbar: logo слева, links по центру, CTA справа (если нет причины)
```

Если поймал себя на одном из этих — останови и переосмысли.

---

## Шаг 3 — Конкретные решения до первой строки кода

Зафикси эти решения в `design-system/tokens.ts` и `design-system/theme.css`:

### Цвет
```
Primary:  [конкретный hex — НЕ дефолтный tailwind]
Neutral:  [тёплый / холодный / чистый серый]
Accent:   [один контрастный цвет или нет вообще]
Mode:     [только light / только dark / обе]
```

### Типографика
```
Display font: [конкретный — не Inter]
Body font:    [конкретный]
Mono font:    [если нужен]
Scale:        [какой размер h1 — 36px? 48px? 60px?]
```

### Пространство
```
Base unit:    [4px или 6px или 8px]
Page padding: [сколько отступ от краёв]
Card padding: [внутренний padding карточек]
```

### Форма
```
Border radius: [острые / мягкие / pill]
Borders:       [есть / нет / только dividers]
Shadows:       [flat / subtle / elevated]
```

---

## Шаг 4 — Референсы по типу проекта

### SaaS / Productivity
Смотри: Linear, Vercel, Raycast, Loom
Приёмы: тёмная тема как основная, mono шрифты для данных,
тонкие бордеры вместо теней, kbd shortcuts везде видны

### Маркетплейс / E-commerce
Смотри: Airbnb, Are.na, Gumroad
Приёмы: фото как главный элемент, минимум хрома вокруг контента,
большие карточки, цена всегда prominent

### Dashboard / Analytics
Смотри: Grafana (новый), Retool, Metabase
Приёмы: плотная сетка, tabular nums везде, цвет только для данных,
sidebar navigation, много состояний данных

### CRM / B2B
Смотри: Attio, Twenty, Notion
Приёмы: таблицы как первый класс UI, inline editing,
команды через keyboard, neutral palette

### Consumer App
Смотри: Arc, Fey, Craft
Приёмы: анимации как часть UX, personality в деталях,
кастомные иконки, деlight моменты

---

## Шаг 5 — Typography as first impression

Самое быстрое решение которое меняет всё — заголовок главной страницы.

```tsx
// ПЛОХО — шаблон:
<h1 className="text-4xl font-bold text-gray-900">
  The platform for modern teams
</h1>

// ХОРОШО — решение принято:
// Выбор: Fraunces (variable, optical sizing) — editorial, с характером
// Размер: clamp(48px, 8vw, 80px) — адаптивный
// Вес: 900 — максимальная выразительность
// Цвет: почти-чёрный с теплотой, не #000000

<h1 className="font-display text-[clamp(3rem,8vw,5rem)]
               font-black tracking-tighter leading-none
               text-neutral-950 dark:text-neutral-50">
  From idea to<br/>
  <span className="text-primary-500">shipped product</span>
</h1>
```

---

## Шаг 6 — Компонентные решения

### Кнопки — не просто rounded
```tsx
// Выбери ОДИН стиль кнопок для всего проекта:

// Стиль А: Flat + border (minimal, professional)
className="border border-neutral-900 bg-transparent px-5 py-2.5
           text-sm font-medium hover:bg-neutral-900 hover:text-white
           transition-colors"

// Стиль Б: Filled + no border (confident, modern)
className="bg-primary-600 text-white px-5 py-2.5 text-sm font-medium
           hover:bg-primary-700 active:scale-[0.98] transition-all"

// Стиль В: Pill (friendly, consumer)
className="rounded-full bg-primary-600 text-white px-6 py-2.5
           text-sm font-medium shadow-sm hover:shadow-md transition-all"

// Стиль Г: Brutal (bold, editorial)
className="bg-neutral-900 text-white px-5 py-2.5 text-sm font-semibold
           border-2 border-neutral-900 hover:bg-transparent
           hover:text-neutral-900 transition-colors"
```

### Карточки — решение один раз
```tsx
// Flat (нет теней, только бордер):
className="rounded-lg border border-neutral-200 dark:border-neutral-800 p-5"

// Elevated (тени вместо бордеров):
className="rounded-xl shadow-md hover:shadow-lg transition-shadow p-5 bg-white"

// Glass (на тёмном фоне):
className="rounded-xl border border-white/10 bg-white/5 backdrop-blur-sm p-5"

// Tinted (цветной фон):
className="rounded-lg bg-primary-50 dark:bg-primary-950/30 p-5"
```

---

## Чеклист перед первым PR

```
[ ] Aesthetic direction зафиксирован (документ или комментарий в tokens.ts)
[ ] Шрифт выбран — не Inter как единственный
[ ] Primary цвет — не дефолтный tailwind синий
[ ] Стиль кнопок выбран — один для всего проекта
[ ] Стиль карточек выбран — один для всего проекта
[ ] Referens-продукт определён ("мы похожи на X но...")
[ ] Dark mode: решено нужен ли с первого дня
[ ] Главный визуальный приём — не более одного
```

## Known pitfalls
- Два "главных визуальных приёма" конфликтуют и убивают оба
- Менять aesthetic direction после 30% разработки — дорого
- "Минимализм" без системы — не минимализм, а незаконченность
- Кастомные шрифты без `font-display: swap` блокируют рендер

## Evolution log
- v1.0: базовый шаблон из bootstrap
