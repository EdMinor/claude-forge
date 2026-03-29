<!-- TRIGGER: If user says "bootstrap" or "start" or "init" — execute this protocol -->
# BOOTSTRAP — Project Initialization Protocol
> **This is a meta-instruction for Claude Code. Do not delete. Read first when starting a project.**

---

## ⚡ First Step — Determine the Path

Before doing anything — check for `TZ.md` in the project root.

```
IF TZ.md exists and contains a project description:
  → Go to Step 1 (Analyze TZ)

IF TZ.md is not found or empty:
  → Launch Skill: Idea Discovery (see below)
  → After TZ.md is generated — return to Step 1
```

---

## 🧭 PATH B — No Spec? Launch Discovery

If TZ.md is not found — tell the user:

```
TZ.md not found. That's fine — I'll help you create it.

Tell me about your idea — however you like:
one sentence, stream of consciousness, with examples.
Don't worry about structure — that's my job.
```

Then follow the protocol from `.claude/skills/idea-discovery/SKILL.md`.
After TZ.md is created — continue as PATH A.

---

## 📋 Step 1 — Analyze TZ.md (PATH A)

Read `TZ.md` fully and determine:

> **Note:** The TZ should follow the structure defined in `.claude/skills/TZ-template/SKILL.md`.
> If the user-provided TZ deviates, map it to this structure before proceeding.

### Project Type
- `web-app` — web application / SPA
- `crm` — CRM / data management
- `ecommerce` — online store / marketplace
- `dashboard` — analytics / dashboards
- `saas` — SaaS tool / platform
- `landing` — landing page / marketing
- `api` — backend API / service
- `fullstack` — full stack

### Stack (from TZ or choose optimal)
If stack is not specified — choose the best fit and explain why.

**Frontend:**
- React + TypeScript + Vite (SPA)
- Next.js (needs SSR, SEO, or API routes)
- Vue 3 + TypeScript (if explicitly in TZ)

**UI:**
- Tailwind CSS v4 (always)
- shadcn/ui (CRM, dashboard, admin, SaaS)
- Headless UI (custom design without pre-built components)

**State + Data:**
- Zustand (UI state)
- TanStack Query (server data)

**Forms:** react-hook-form + zod

**Routing:**
- React Router v7 (default for Vite SPAs — mature, well-documented)
- TanStack Router (type-safe routing, if project is heavily TypeScript-first)
- Next.js built-in routing (if Next.js is chosen)

**By project type:**
- CRM/Dashboard: recharts, @tanstack/react-table, @dnd-kit/core
- With maps: react-leaflet
- With animations: framer-motion (always)
- E-commerce: stripe-js, @stripe/react-stripe-js

### Project Modules
List from TZ — each module = a separate subagent later.

---

## 🏗️ Step 2 — Create Project Structure

### 2.1 Initialization

```bash
# React + Vite:
npm create vite@latest . -- --template react-ts && npm install

# Tailwind CSS v4:
npm install tailwindcss @tailwindcss/vite

# Core dependencies:
npm install zustand @tanstack/react-query react-hook-form zod @hookform/resolvers
npm install lucide-react framer-motion sonner
npm install -D @types/node

# Router (choose one based on Step 1):
npm install react-router        # React Router v7
# OR
npm install @tanstack/react-router  # TanStack Router

# shadcn/ui (for CRM / dashboard / SaaS):
npx shadcn@latest init

# Fonts (choose for the project):
npm install @fontsource-variable/plus-jakarta-sans
npm install @fontsource-variable/dm-sans
npm install @fontsource/jetbrains-mono
```

### 2.2 src/ Structure

```
src/
├── features/           ← one TZ module = one folder
│   ├── [module-1]/
│   │   ├── index.tsx
│   │   ├── components/
│   │   ├── hooks/
│   │   └── types.ts
│   └── [module-N]/
├── components/
│   ├── ui/             ← shadcn + base components
│   └── shared/         ← reusable across project
├── design-system/
│   ├── tokens.ts       ← colors, typography, spacing
│   └── theme.css       ← CSS variables
├── api/
│   ├── client.ts       ← axios / fetch config
│   └── [module].ts     ← one file per module
├── stores/             ← zustand stores
├── hooks/              ← global hooks
├── types/              ← global types
├── utils/
│   └── cn.ts           ← classnames utility
├── router/
│   └── index.tsx       ← route definitions (React Router or TanStack Router)
└── main.tsx
```

---

## 🤖 Step 3 — Create Agent Infrastructure

### 3.1 .claude/ Structure

```
.claude/
├── CLAUDE.md                    ← CREATE FIRST
├── settings.json                ← hooks and permissions (see 3.6)
├── skills/
│   ├── idea-discovery/SKILL.md  ← always (Path B)
│   ├── design-tokens/SKILL.md   ← always
│   ├── typography/SKILL.md      ← always
│   ├── frontend-design/SKILL.md ← always
│   ├── layout-craft/SKILL.md    ← always
│   ├── ux-states/SKILL.md       ← always
│   ├── motion/SKILL.md          ← always
│   ├── ui-components/SKILL.md   ← generate for project (see 3.4)
│   ├── api-patterns/SKILL.md    ← generate for project (see 3.4)
│   ├── testing/SKILL.md         ← generate for project (see 3.4)
│   └── [domain]/SKILL.md        ← generate for project type (see 3.5)
├── agents/
│   ├── orchestrator.md          ← with YAML frontmatter
│   ├── [module]-agent.md        ← one per module, with frontmatter
│   └── qa-agent.md              ← quality assurance agent
└── docs/
    ├── context/
    │   └── current-context.md   ← update every session
    ├── daily/
    ├── decisions/               ← ADR
    ├── errors/
    │   └── solved/
    ├── weekly/
    └── milestones/
```

### 3.2 CLAUDE.md — Contents

```markdown
# [Project Name] — Claude Instructions

## Project
[Summary from TZ — 2-3 sentences]

## Stack
[Full stack with versions]

## Modules
[List of all modules from TZ]

## Development Rules
- Components: functional, TypeScript, strict typing
- Never use `any`
- Each module — isolated folder features/[module]/
- API requests only through src/api/
- Forms only through react-hook-form + zod
- Zustand for UI state, TanStack Query for server state

## Design — read BEFORE first component
1. .claude/skills/design-tokens/     ← palette and fonts
2. .claude/skills/typography/        ← text hierarchy
3. .claude/skills/frontend-design/   ← aesthetic direction
4. .claude/skills/layout-craft/      ← per screen
5. .claude/skills/ux-states/         ← loading/empty/error
6. .claude/skills/motion/            ← after base structure

## Documentation
- Update current-context.md after every session
- ADR for architectural decisions
- Log solved errors in errors/solved/
```

### 3.3 Agents

**orchestrator.md:**
```markdown
---
name: orchestrator
description: Master agent that coordinates all module agents and maintains project consistency
model: inherit
memory: project
skills:
  - design-tokens
  - typography
  - frontend-design
---

# Orchestrator Agent

## Session Protocol
1. Read current-context.md
2. Read CLAUDE.md
3. Clarify the task
4. Launch subagents (in parallel for independent modules)
5. Collect results, verify UI consistency
6. Update current-context.md

## Parallelization Rules
- Design system → always first, blocks others
- Independent modules → in parallel
- Module with dependencies → after dependencies
```

**For each module from TZ:**
```markdown
---
name: [module]-agent
description: Handles development of the [module] feature module
model: inherit
memory: project
skills:
  - ui-components
  - ux-states
  - [domain]
---

# Agent: [Module]

## Responsibility
[What this agent does]

## Read Before Working
- .claude/skills/ui-components/
- .claude/skills/ux-states/
- .claude/skills/[domain]/

## Input
- API: [endpoints from TZ]
- Depends on: [other modules]

## Output
- src/features/[module]/
- src/api/[module].ts

## Done Criteria
[ ] All components typed
[ ] Loading / empty / error states
[ ] API hooks via TanStack Query
[ ] Responsive layout
[ ] Hover states on interactive elements
```

### 3.4 Project-Specific Skills — Generation Instructions

After determining the stack and modules in Step 1, generate these skills based on the project:

**ui-components/ (SKILL.md):**
Generate based on the chosen UI framework. Include:
- Component naming conventions and file structure
- How to extend shadcn/ui components (if used)
- Project-specific composite components (e.g., DataTable for CRM, ProductCard for e-commerce)
- Prop patterns: prefer composition over configuration
- When to create a shared component vs. feature-local component

**api-patterns/ (SKILL.md):**
Generate based on the data layer. Include:
- API client setup (base URL, interceptors, auth headers)
- TanStack Query hook patterns: queries, mutations, optimistic updates
- Error handling strategy (toast on mutation fail, error boundary on query fail)
- Type generation approach (manual types from TZ or auto-generated from OpenAPI)
- Cache invalidation patterns per module

**testing/ (SKILL.md):**
Generate based on project type and complexity. Include:
- Test runner choice (Vitest for Vite projects)
- What to test: business logic in hooks, form validation schemas, API transforms
- What NOT to test: pure UI rendering, third-party library internals
- Testing patterns: arrange-act-assert, mock API with MSW
- Minimum coverage targets per module

### 3.5 Domain-Specific Skills — Generation by Project Type

Generate ONE domain skill matching the project type from Step 1:

**For CRM (`crm/SKILL.md`):**
- Data table patterns (sorting, filtering, pagination, column visibility)
- Form-heavy workflows (multi-step, draft saving, validation)
- Entity relationship display (linked records, breadcrumbs)
- Bulk operations (select all, batch update/delete)
- Search and filter UX patterns

**For E-commerce (`ecommerce/SKILL.md`):**
- Product display patterns (grid, list, gallery, quick view)
- Cart and checkout flow (state management, step validation)
- Payment integration (Stripe Elements, error handling)
- Inventory and pricing display (stock status, sale badges)
- Category navigation and filtering

**For Dashboard (`dashboard/SKILL.md`):**
- Chart composition (recharts patterns, responsive containers)
- KPI cards and metric display (trend indicators, sparklines)
- Date range selection and data filtering
- Real-time data updates (polling, WebSocket patterns)
- Export functionality (CSV, PDF)

**For SaaS (`saas/SKILL.md`):**
- Multi-tenancy patterns (workspace switching, data isolation)
- Subscription and billing UI (plan comparison, upgrade flows)
- Settings and configuration pages
- Onboarding flows (progressive disclosure, empty states with CTAs)
- Role-based access display (show/hide based on permissions)

**For Landing (`landing/SKILL.md`):**
- Hero section patterns (split, centered, video background)
- Social proof sections (testimonials, logos, stats)
- CTA optimization (placement, urgency, contrast)
- FAQ and feature comparison sections
- Performance optimization (lazy images, above-the-fold priority)

### 3.6 QA Agent

```markdown
---
name: qa-agent
description: Reviews completed modules for type safety, UX states coverage, responsive design, and design system compliance
model: inherit
memory: project
skills:
  - ux-states
  - design-tokens
  - frontend-design
---

# QA Agent

## Review Checklist (per module)
- [ ] All components use TypeScript strict mode, no `any`
- [ ] All data-fetching states: loading (skeleton) / error / empty / success
- [ ] Responsive: tested at 375px, 768px, 1280px
- [ ] Design tokens used consistently (no hardcoded colors/spacing)
- [ ] Interactive elements have hover/focus/active states
- [ ] Accessibility: semantic HTML, alt texts, keyboard navigation
- [ ] No console errors or warnings

## How to Report
Create a review file at `.claude/docs/reviews/[module]-review.md` with:
- Issues found (severity: critical/major/minor)
- Suggestions for improvement
- Compliance score (0-100%)
```

### 3.7 Settings with Hooks

Generate `.claude/settings.json` to enforce quality gates:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm run *)",
      "Bash(npx *)",
      "Bash(git *)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx tsc --noEmit --pretty 2>&1 | head -20",
            "timeout": 30000
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Remember: update .claude/docs/context/current-context.md if this was a significant session'"
          }
        ]
      }
    ]
  }
}
```

> **Note:** Adapt hooks based on the project's linter/formatter setup.
> Add `eslint`, `prettier`, or test commands as they become available.

---

## 📝 Step 4 — Startup Documentation

### current-context.md (initial)

```markdown
# Current Context
Last updated: [date]

## Status
Phase: Bootstrap / Init | Readiness: 0%

## Progress
- [x] Bootstrap completed
- [x] TZ.md created / exists
- [ ] Design system (tokens + typography)
- [ ] [Module 1]
- [ ] [Module N]

## Next Step
Create design system: tokens.ts + theme.css

## Open Questions from TZ
[Questions that need clarification]

## Blockers
None
```

### M0-bootstrap.md

```markdown
# Milestone 0 — Bootstrap ✅

## Date: [date]

## Done
- TZ.md: [created via Discovery / provided]
- Stack: [final choice]
- Structure created
- CLAUDE.md, skills, agents ready
- Documentation launched

## Next Milestone
M1 — Design System and Auth
```

---

## ⚡ Step 5 — Final Check

```
[ ] TZ.md exists and is filled
[ ] Stack chosen and justified
[ ] npm install completed without critical errors
[ ] .claude/CLAUDE.md created
[ ] .claude/skills/ — all skills (design + project-specific)
[ ] .claude/agents/ — orchestrator + module agents
[ ] .claude/docs/context/current-context.md created
[ ] .claude/docs/milestones/M0-bootstrap.md created
[ ] src/ structure created
[ ] README.md created
```

If everything is ready — tell the user:
```
Bootstrap complete ✓

Project: [name]
Stack: [stack]
Modules: [list]

Next step: [specific first task from TZ]
Start now?
```

---

## 🔄 Skill Update Protocol

After each completed module:
1. What new patterns did we learn about this project?
2. What errors were solved?
3. What should be added to prohibited list?
4. Update skill version + evolution log

**Update triggers:**
- Module completed
- Non-trivial bug solved → `errors/solved/ERR-NNN.md`
- Architectural decision → `decisions/ADR-NNN.md`
- Week passed → `weekly/week-XX.md`

---

## 📐 Document Templates

### ADR
`.claude/docs/decisions/ADR-NNN-[name].md`
```markdown
# ADR-NNN: [Decision]
## Status: Accepted | Date: [date]
## Context
## Options Considered
## Decision and Rationale
## Consequences
```

### Daily Log
`.claude/docs/daily/YYYY-MM-DD.md`
```markdown
# Daily — YYYY-MM-DD
## Completed
## Decisions Made Today
## Errors and Solutions
## Tomorrow
```

### Error Encyclopedia
`.claude/docs/errors/solved/ERR-NNN-[name].md`
```markdown
# ERR-NNN: [Error]
## Symptom
## Cause
## Solution (code/steps)
## Prevention → add to skill
```

---

## 🎨 Design Skills — Application Order

```
Before first component:
  1. design-tokens/   → palette, fonts, spacing
  2. typography/       → hierarchy, details
  3. frontend-design/  → aesthetic direction

Per screen:
  4. layout-craft/     → hierarchy, grid, breathing room
  5. ux-states/        → loading / empty / error

After base structure:
  6. motion/           → transitions, micro-interactions
```

### "Not a Template" Checklist
```
[ ] Color: not default shadcn blue #3b82f6
[ ] Font: not only Inter
[ ] Loading: no bare <Spinner /> at page level
[ ] Empty: no "No data found" without icon and action
[ ] Hover: every card reacts
[ ] Numbers: tabular-nums on statistics
[ ] Page transitions: work between routes
[ ] Sticky header: backdrop-blur
[ ] Dark mode: verified on all states
```

---

> **Protocol version:** 2.0
> **Compatibility:** Claude Code, Claude Sonnet 4+
