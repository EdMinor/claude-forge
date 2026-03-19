<!-- TRIGGER: If user says "bootstrap" or "start" or "init" — execute this protocol -->
# BOOTSTRAP — Project Initialization Protocol
> **Это мета-инструкция для Claude Code. Не удалять. Читать первым делом при старте проекта.**

---

## ⚡ Первый шаг — Определи путь

Прежде чем что-либо делать — проверь наличие `TZ.md` в корне проекта.

```
ЕСЛИ TZ.md существует и содержит описание проекта:
  → Переходи к Шагу 1 (Анализ TZ)
  
ЕСЛИ TZ.md не найден или пустой:
  → Запусти Skill: Idea Discovery (см. ниже)
  → После генерации TZ.md — возвращайся к Шагу 1
```

---

## 🧭 PATH B — Нет ТЗ? Запусти Discovery

Если TZ.md не найден — скажи пользователю:

```
TZ.md не найден. Это нормально — я помогу его создать.

Расскажи мне о своей идее — можно как угодно:
одним предложением, потоком сознания, с примерами.
Не думай о структуре — это моя работа.
```

Затем следуй протоколу из `.claude/skills/idea-discovery.md`.
После того как TZ.md создан — продолжай как PATH A.

---

## 📋 Шаг 1 — Анализ TZ.md (PATH A)

Прочитай `TZ.md` полностью и определи:

### Тип проекта
- `web-app` — веб-приложение / SPA
- `crm` — CRM / управление данными
- `ecommerce` — интернет-магазин / маркетплейс
- `dashboard` — аналитика / дашборды
- `saas` — SaaS инструмент / платформа
- `landing` — лендинг / маркетинг
- `api` — backend API / сервис
- `fullstack` — полный стек

### Стек (из TZ или выбери оптимальный)
Если стек не указан — выбери подходящий и укажи почему.

**Frontend:**
- React + TypeScript + Vite (SPA)
- Next.js (нужен SSR, SEO или API routes)
- Vue 3 + TypeScript (если явно в ТЗ)

**UI:**
- Tailwind CSS v4 (всегда)
- shadcn/ui (CRM, dashboard, admin, SaaS)
- Headless UI (кастомный дизайн без готовых компонентов)

**Стейт + данные:**
- Zustand (UI-стейт)
- TanStack Query (серверные данные)

**Формы:** react-hook-form + zod

**По типу проекта:**
- CRM/Dashboard: recharts, @tanstack/react-table, @dnd-kit/core
- С картами: react-leaflet
- С анимациями: framer-motion (всегда)
- E-commerce: stripe-js, @stripe/react-stripe-js

### Модули проекта
Список из TZ — каждый модуль = отдельный субагент позже.

---

## 🏗️ Шаг 2 — Создай структуру проекта

### 2.1 Инициализация

```bash
# React + Vite:
npm create vite@latest . -- --template react-ts && npm install

# Tailwind CSS v4:
npm install tailwindcss @tailwindcss/vite

# Базовые зависимости:
npm install zustand @tanstack/react-query react-hook-form zod @hookform/resolvers
npm install lucide-react framer-motion sonner
npm install -D @types/node

# shadcn/ui (для CRM / dashboard / SaaS):
npx shadcn@latest init

# Шрифты (выбери под проект):
npm install @fontsource-variable/plus-jakarta-sans
npm install @fontsource-variable/dm-sans
npm install @fontsource/jetbrains-mono
```

### 2.2 Структура src/

```
src/
├── features/           ← один модуль из TZ = одна папка
│   ├── [module-1]/
│   │   ├── index.tsx
│   │   ├── components/
│   │   ├── hooks/
│   │   └── types.ts
│   └── [module-N]/
├── components/
│   ├── ui/             ← shadcn + базовые компоненты
│   └── shared/         ← переиспользуемые в проекте
├── design-system/
│   ├── tokens.ts       ← цвета, типографика, spacing
│   └── theme.css       ← CSS переменные
├── api/
│   ├── client.ts       ← axios / fetch конфиг
│   └── [module].ts     ← по файлу на каждый модуль
├── stores/             ← zustand stores
├── hooks/              ← глобальные хуки
├── types/              ← глобальные типы
├── utils/
│   └── cn.ts           ← classnames utility
├── router/
│   └── index.tsx
└── main.tsx
```

---

## 🤖 Шаг 3 — Создай агентную инфраструктуру

### 3.1 Структура .claude/

```
.claude/
├── CLAUDE.md                    ← СОЗДАТЬ ПЕРВЫМ
├── skills/
│   ├── idea-discovery.md        ← всегда (Path B)
│   ├── design-tokens.md         ← всегда
│   ├── typography.md            ← всегда
│   ├── frontend-design.md       ← всегда
│   ├── layout-craft.md          ← всегда
│   ├── ux-states.md             ← всегда
│   ├── motion.md                ← всегда
│   ├── ui-components.md         ← генерируется под проект
│   ├── api-patterns.md          ← генерируется под проект
│   ├── testing.md               ← генерируется под проект
│   └── [domain].md              ← crm / ecommerce / saas / etc
├── agents/
│   ├── orchestrator.md
│   ├── [module]-agent.md        ← по одному на каждый модуль
│   └── qa-agent.md
└── docs/
    ├── context/
    │   └── current-context.md   ← обновлять каждую сессию
    ├── daily/
    ├── decisions/               ← ADR
    ├── errors/
    │   └── solved/
    ├── weekly/
    └── milestones/
```

### 3.2 CLAUDE.md — содержимое

```markdown
# [Название проекта] — Claude Instructions

## Проект
[Суть из TZ — 2-3 предложения]

## Стек
[Полный стек с версиями]

## Модули
[Список всех модулей из TZ]

## Правила разработки
- Компоненты: функциональные, TypeScript, строгая типизация
- Никогда не использовать `any`
- Каждый модуль — изолированная папка features/[module]/
- API-запросы только через src/api/
- Формы только через react-hook-form + zod
- Zustand для UI-стейта, TanStack Query для серверного

## Дизайн — читать ПЕРЕД первым компонентом
1. .claude/skills/design-tokens.md     ← палитра и шрифты
2. .claude/skills/typography.md        ← иерархия текста
3. .claude/skills/frontend-design.md  ← aesthetic direction
4. .claude/skills/layout-craft.md     ← при каждом экране
5. .claude/skills/ux-states.md        ← loading/empty/error
6. .claude/skills/motion.md           ← после базовой структуры

## Документация
- Обновляй current-context.md после каждой сессии
- ADR при архитектурных решениях
- Логируй решённые ошибки в errors/solved/
```

### 3.3 Агенты

**orchestrator.md:**
```markdown
# Orchestrator Agent

## Протокол сессии
1. Прочитать current-context.md
2. Прочитать CLAUDE.md
3. Уточнить задачу
4. Запустить субагентов (параллельно для независимых модулей)
5. Собрать результаты, проверить консистентность UI
6. Обновить current-context.md

## Правила параллелизации
- Дизайн-система → всегда первая, блокирует остальных
- Независимые модули → параллельно
- Модуль с зависимостями → после зависимостей
```

**Для каждого модуля из TZ:**
```markdown
# Agent: [Module]

## Ответственность
[Что делает этот агент]

## Читать перед работой
- .claude/skills/ui-components.md
- .claude/skills/ux-states.md
- .claude/skills/[domain].md

## Входные данные
- API: [эндпоинты из TZ]
- Зависит от: [другие модули]

## Выходные данные
- src/features/[module]/
- src/api/[module].ts

## Критерии готовности
[ ] Все компоненты типизированы
[ ] Loading / empty / error states
[ ] API-хуки через TanStack Query
[ ] Адаптивная вёрстка
[ ] Hover states на интерактивных элементах
```

---

## 📝 Шаг 4 — Документация старта

### current-context.md (начальная)

```markdown
# Current Context
Last updated: [дата]

## Статус
Фаза: Bootstrap / Init | Готовность: 0%

## Прогресс
- [x] Bootstrap завершён
- [x] TZ.md создан / существует
- [ ] Дизайн-система (tokens + typography)
- [ ] [Модуль 1]
- [ ] [Модуль N]

## Следующий шаг
Создать дизайн-систему: tokens.ts + theme.css

## Открытые вопросы из TZ
[Вопросы которые нужно уточнить]

## Блокеры
Нет
```

### M0-bootstrap.md

```markdown
# Milestone 0 — Bootstrap ✅

## Дата: [дата]

## Сделано
- TZ.md: [создан через Discovery / предоставлен]
- Стек: [итоговый выбор]
- Структура создана
- CLAUDE.md, скиллы, агенты готовы
- Документация запущена

## Следующий milestone
M1 — Дизайн-система и Auth
```

---

## ⚡ Шаг 5 — Финальная проверка

```
[ ] TZ.md существует и заполнен
[ ] Стек выбран и обоснован
[ ] npm install прошёл без критических ошибок
[ ] .claude/CLAUDE.md создан
[ ] .claude/skills/ — все скиллы (дизайн + проектные)
[ ] .claude/agents/ — оркестратор + модульные агенты
[ ] .claude/docs/context/current-context.md создан
[ ] .claude/docs/milestones/M0-bootstrap.md создан
[ ] src/ структура создана
[ ] README.md создан
```

Если всё готово — скажи пользователю:
```
Bootstrap завершён ✓

Проект: [название]
Стек: [стек]
Модули: [список]

Следующий шаг: [конкретная первая задача из TZ]
Запустить сейчас?
```

---

## 🔄 Протокол обновления скиллов

После каждого завершённого модуля:
1. Что нового узнали о паттернах проекта?
2. Какие ошибки были решены?
3. Что добавить в запрещённое?
4. Обновить версию скилла + evolution log

**Триггеры обновления:**
- Завершён модуль
- Решён нетривиальный баг → `errors/solved/ERR-NNN.md`
- Архитектурное решение → `decisions/ADR-NNN.md`
- Прошла неделя → `weekly/week-XX.md`

---

## 📐 Шаблоны документов

### ADR
`.claude/docs/decisions/ADR-NNN-[название].md`
```markdown
# ADR-NNN: [Решение]
## Статус: Accepted | Дата: [дата]
## Контекст
## Варианты рассмотрены
## Решение и обоснование
## Последствия
```

### Daily Log
`.claude/docs/daily/YYYY-MM-DD.md`
```markdown
# Daily — YYYY-MM-DD
## Выполнено
## Решения принятые сегодня
## Ошибки и решения
## Завтра
```

### Error Encyclopedia
`.claude/docs/errors/solved/ERR-NNN-[название].md`
```markdown
# ERR-NNN: [Ошибка]
## Симптом
## Причина
## Решение (код/шаги)
## Профилактика → добавить в скилл
```

---

## 🎨 Дизайн-скиллы — порядок применения

```
До первого компонента:
  1. design-tokens.md   → палитра, шрифты, spacing
  2. typography.md      → иерархия, детали
  3. frontend-design.md → aesthetic direction

При каждом экране:
  4. layout-craft.md    → hierarchy, grid, breathing room
  5. ux-states.md       → loading / empty / error

После базовой структуры:
  6. motion.md          → transitions, micro-interactions
```

### Чеклист "не шаблон"
```
[ ] Цвет: не дефолтный синий shadcn #3b82f6
[ ] Шрифт: не только Inter
[ ] Loading: нет голого <Spinner /> на уровне страницы
[ ] Empty: не "No data found" без иконки и action
[ ] Hover: каждая карточка реагирует
[ ] Numbers: tabular-nums на статистике
[ ] Page transitions: работают между роутами
[ ] Sticky header: backdrop-blur
[ ] Dark mode: проверено на всех состояниях
```

---

> **Версия протокола:** 2.0 (добавлен Path B + Discovery Agent + полный TZ формат)
> **Совместимость:** Claude Code, Claude Sonnet 4+
