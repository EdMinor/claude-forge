# Skill: UX States
Version: 1.0 | Layer: Polish — самый игнорируемый, самый заметный

## Зачем этот скилл
Шаблонный проект: спиннер в центре экрана + "No data found" + красный алерт.
Продвинутый проект: skeleton с ритмом, пустые состояния с характером, тосты с контекстом.
Это то, что пользователь видит постоянно. Именно здесь шаблон виден сразу.

---

## Правило 1 — Никаких спиннеров на уровне страницы

```tsx
// ПЛОХО — шаблонный паттерн:
if (isLoading) return <Spinner />

// ХОРОШО — skeleton повторяет форму реального контента:
if (isLoading) return <DashboardSkeleton />
```

### Skeleton компонент (базовый)

```tsx
// src/components/ui/Skeleton.tsx
import { cn } from '@/utils/cn'

export function Skeleton({ className }: { className?: string }) {
  return (
    <div
      className={cn(
        'animate-pulse rounded-md bg-neutral-200 dark:bg-neutral-800',
        className
      )}
    />
  )
}

// Анимация в tailwind.config:
// animation: { pulse: 'pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite' }
// Используй разные задержки для ритма:
// <Skeleton className="h-4 w-3/4 animation-delay-75" />
// <Skeleton className="h-4 w-1/2 animation-delay-150" />
```

### Skeleton для типичных layouts

```tsx
// Карточка с заголовком и текстом:
export function CardSkeleton() {
  return (
    <div className="rounded-lg border border-neutral-200 p-4 space-y-3">
      <Skeleton className="h-5 w-2/3" />
      <Skeleton className="h-4 w-full" />
      <Skeleton className="h-4 w-4/5" />
      <div className="flex gap-2 pt-2">
        <Skeleton className="h-8 w-20" />
        <Skeleton className="h-8 w-16" />
      </div>
    </div>
  )
}

// Строка таблицы:
export function TableRowSkeleton() {
  return (
    <tr>
      <td className="py-3 px-4"><Skeleton className="h-4 w-8" /></td>
      <td className="py-3 px-4"><Skeleton className="h-4 w-32" /></td>
      <td className="py-3 px-4"><Skeleton className="h-4 w-24" /></td>
      <td className="py-3 px-4"><Skeleton className="h-4 w-16" /></td>
    </tr>
  )
}

// Аватар + имя:
export function UserSkeleton() {
  return (
    <div className="flex items-center gap-3">
      <Skeleton className="h-10 w-10 rounded-full" />
      <div className="space-y-2">
        <Skeleton className="h-4 w-24" />
        <Skeleton className="h-3 w-16" />
      </div>
    </div>
  )
}
```

---

## Правило 2 — Empty States с характером

Empty state — это возможность, не ошибка. Должен:
1. Объяснить что здесь будет
2. Дать конкретное действие
3. Иметь иллюстрацию или иконку (не generic)

```tsx
// src/components/ui/EmptyState.tsx
interface EmptyStateProps {
  icon?: React.ReactNode
  title: string
  description: string
  action?: {
    label: string
    onClick: () => void
  }
}

export function EmptyState({ icon, title, description, action }: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center py-16 px-4 text-center">
      {icon && (
        <div className="mb-4 rounded-full bg-neutral-100 dark:bg-neutral-800 p-4 text-neutral-400">
          {icon}
        </div>
      )}
      <h3 className="text-base font-semibold text-neutral-900 dark:text-neutral-100 mb-1">
        {title}
      </h3>
      <p className="text-sm text-neutral-500 dark:text-neutral-400 max-w-sm mb-6">
        {description}
      </p>
      {action && (
        <button
          onClick={action.onClick}
          className="rounded-md bg-primary-600 px-4 py-2 text-sm font-medium text-white
                     hover:bg-primary-700 transition-colors"
        >
          {action.label}
        </button>
      )}
    </div>
  )
}

// Использование — конкретные, не generic:
<EmptyState
  icon={<SearchIcon className="w-6 h-6" />}
  title="Ничего не найдено"
  description="Попробуй другой запрос или сними фильтры"
  action={{ label: 'Сбросить фильтры', onClick: clearFilters }}
/>

// НЕ ДЕЛАЙ ТАК:
// title="No data"
// description="No items found"
```

---

## Правило 3 — Toast система (не браузерный alert)

```tsx
// Используй sonner — лучшая toast библиотека
// npm install sonner

// src/main.tsx:
import { Toaster } from 'sonner'
<Toaster position="bottom-right" richColors />

// Использование с контекстом — не просто "Success":
import { toast } from 'sonner'

// ПЛОХО:
toast.success('Success')
toast.error('Error')

// ХОРОШО — конкретно и с действием:
toast.success('Клиент добавлен', {
  description: 'Иван Петров добавлен в базу',
  action: {
    label: 'Открыть',
    onClick: () => navigate(`/clients/${id}`)
  }
})

toast.error('Не удалось сохранить', {
  description: 'Проверь подключение к интернету',
  action: {
    label: 'Повторить',
    onClick: retry
  }
})
```

---

## Правило 4 — Error States (не просто красный блок)

```tsx
// src/components/ui/ErrorState.tsx
interface ErrorStateProps {
  title?: string
  message?: string
  retry?: () => void
  variant?: 'page' | 'inline' | 'card'
}

export function ErrorState({
  title = 'Что-то пошло не так',
  message = 'Попробуй обновить страницу',
  retry,
  variant = 'inline'
}: ErrorStateProps) {
  if (variant === 'page') {
    return (
      <div className="min-h-[400px] flex flex-col items-center justify-center gap-4">
        <div className="text-4xl">⚠️</div>
        <h2 className="text-xl font-semibold">{title}</h2>
        <p className="text-sm text-neutral-500">{message}</p>
        {retry && (
          <button onClick={retry} className="...">Попробовать снова</button>
        )}
      </div>
    )
  }

  return (
    <div className="rounded-lg border border-red-200 bg-red-50 dark:bg-red-950/20
                    dark:border-red-900 px-4 py-3 text-sm">
      <p className="font-medium text-red-800 dark:text-red-400">{title}</p>
      {message && (
        <p className="mt-1 text-red-700 dark:text-red-500">{message}</p>
      )}
    </div>
  )
}
```

---

## Правило 5 — Optimistic Updates

```tsx
// Не жди ответа сервера для простых действий
// Используй TanStack Query useMutation:

const { mutate: toggleFavorite } = useMutation({
  mutationFn: api.toggleFavorite,
  onMutate: async (id) => {
    // Сразу обновляем UI
    await queryClient.cancelQueries({ queryKey: ['listings'] })
    const prev = queryClient.getQueryData(['listings'])
    queryClient.setQueryData(['listings'], (old) =>
      old.map(item => item.id === id
        ? { ...item, isFavorite: !item.isFavorite }
        : item
      )
    )
    return { prev }
  },
  onError: (err, id, ctx) => {
    // Откатываем если ошибка
    queryClient.setQueryData(['listings'], ctx.prev)
    toast.error('Не удалось сохранить')
  }
})
```

---

## Чеклист перед сдачей модуля

```
[ ] Все data-fetching состояния покрыты: loading / error / empty / success
[ ] Нет ни одного голого <Spinner /> на уровне страницы
[ ] Каждый empty state имеет title + description + action (если уместно)
[ ] Toast-ы конкретные, не generic ("Сохранено" → "Объект #123 сохранён")
[ ] Error states разного уровня: page / card / inline
[ ] Optimistic updates для toggle/like/favorite операций
```

## Known pitfalls
- skeleton анимация `animate-pulse` на тёмном фоне — используй `dark:bg-neutral-700`
- sonner и shadcn Toast — выбери одно, не оба
- Optimistic update: всегда сохраняй `prev` для rollback

## Evolution log
- v1.0: базовый шаблон из bootstrap
