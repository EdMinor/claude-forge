---
name: ux-states
description: Polish layer — skeleton screens, empty states with character, toast system, error states, optimistic updates. Apply per screen.
user-invocable: false
---

# Skill: UX States
Version: 1.0 | Layer: Polish — the most ignored, the most noticeable

## Why this skill
Template project: spinner in the center of the screen + "No data found" + red alert.
Advanced project: skeleton with rhythm, empty states with character, toasts with context.
This is what the user sees constantly. This is where the template shows immediately.

---

## Rule 1 — No page-level spinners

```tsx
// BAD — template pattern:
if (isLoading) return <Spinner />

// GOOD — skeleton mirrors the shape of real content:
if (isLoading) return <DashboardSkeleton />
```

### Skeleton component (basic)

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

// Animation in tailwind.config:
// animation: { pulse: 'pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite' }
// Use different delays for rhythm:
// <Skeleton className="h-4 w-3/4 animation-delay-75" />
// <Skeleton className="h-4 w-1/2 animation-delay-150" />
```

### Skeletons for common layouts

```tsx
// Card with title and text:
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

// Table row:
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

// Avatar + name:
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

## Rule 2 — Empty States with character

An empty state is an opportunity, not an error. It should:
1. Explain what will be here
2. Give a concrete action
3. Have an illustration or icon (not generic)

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

// Usage — specific, not generic:
<EmptyState
  icon={<SearchIcon className="w-6 h-6" />}
  title="Nothing found"
  description="Try a different query or remove filters"
  action={{ label: 'Reset filters', onClick: clearFilters }}
/>

// DON'T DO THIS:
// title="No data"
// description="No items found"
```

---

## Rule 3 — Toast system (not browser alert)

```tsx
// Use sonner — the best toast library
// npm install sonner

// src/main.tsx:
import { Toaster } from 'sonner'
<Toaster position="bottom-right" richColors />

// Usage with context — not just "Success":
import { toast } from 'sonner'

// BAD:
toast.success('Success')
toast.error('Error')

// GOOD — specific and with action:
toast.success('Client added', {
  description: 'John Smith has been added to the database',
  action: {
    label: 'Open',
    onClick: () => navigate(`/clients/${id}`)
  }
})

toast.error('Could not save', {
  description: 'Check your internet connection',
  action: {
    label: 'Retry',
    onClick: retry
  }
})
```

---

## Rule 4 — Error States (not just a red block)

```tsx
// src/components/ui/ErrorState.tsx
interface ErrorStateProps {
  title?: string
  message?: string
  retry?: () => void
  variant?: 'page' | 'inline' | 'card'
}

export function ErrorState({
  title = 'Something went wrong',
  message = 'Try refreshing the page',
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
          <button onClick={retry} className="...">Try again</button>
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

## Rule 5 — Optimistic Updates

```tsx
// Don't wait for the server response for simple actions
// Use TanStack Query useMutation:

const { mutate: toggleFavorite } = useMutation({
  mutationFn: api.toggleFavorite,
  onMutate: async (id) => {
    // Update UI immediately
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
    // Rollback on error
    queryClient.setQueryData(['listings'], ctx.prev)
    toast.error('Could not save')
  }
})
```

---

## Checklist before module delivery

```
[ ] All data-fetching states covered: loading / error / empty / success
[ ] No bare <Spinner /> at page level
[ ] Every empty state has title + description + action (where appropriate)
[ ] Toasts are specific, not generic ("Saved" → "Object #123 saved")
[ ] Error states at different levels: page / card / inline
[ ] Optimistic updates for toggle/like/favorite operations
```

## Known pitfalls
- skeleton animation `animate-pulse` on dark background — use `dark:bg-neutral-700`
- sonner and shadcn Toast — pick one, not both
- Optimistic update: always save `prev` for rollback

## Evolution log
- v1.0: initial template from bootstrap
