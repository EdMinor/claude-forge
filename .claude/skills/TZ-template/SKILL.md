---
name: TZ-template
description: Project specification template — structured format for TZ.md with vision, audience, market, phases, stack, and analytics sections.
user-invocable: false
---

# TZ Template — Project Specification Format
> This file is a template for generating TZ.md.
> Filled by the idea-discovery agent or manually by the user.

---

# [Project Name]
> Working title. Can be changed later.

**Version:** 1.0
**Date:** [date]
**Status:** Draft / Approved
**Author:** [name] + Claude Discovery Agent

---

## 🎯 Vision

### Product Essence
[1-2 sentences. What it is, for whom, what problem it solves.]

### Problem
[Specific user pain. Why existing solutions are insufficient.]

### Solution
[How exactly the product solves this problem. Key mechanism.]

### Unique Advantage
[What differentiates from competitors. Not "better and cheaper" — be specific.]

---

## 👥 Audience

### Core Personas

#### Persona 1: [Name / Role]
```
Who:        [description]
Task:       [what they want to accomplish]
Pain:       [what currently doesn't work]
Expectation:[what the product should deliver]
Frequency:  [how often they'll use it]
```

#### Persona 2: [Name / Role]
```
Who:        [description]
Task:       [what they want to accomplish]
Pain:       [what currently doesn't work]
Expectation:[what the product should deliver]
Frequency:  [how often they'll use it]
```

### Who is NOT our audience
[Who we consciously don't serve at this stage]

---

## 🗺️ Market Context

### Competitors
| Product | What they do well | Where they're weak | Price |
|---------|------------------|-------------------|-------|
| [Comp. 1] | | | |
| [Comp. 2] | | | |
| [Comp. 3] | | | |

### Our Positioning
[Where we are in the market relative to competitors]

### Market Size (if known)
[TAM / SAM / SOM or just context]

---

## 💰 Monetization

### Model
[Subscription / Commission / Freemium / One-time / Ads / Hybrid]

### Pricing (plan)
```
Free tier:    [what's included, if applicable]
Basic:        [$X/mo — what's included]
Pro:          [$X/mo — what's included]
Enterprise:   [custom / on request]
```

### Success Metrics
```
MRR target (3 mo):    [X]
MRR target (12 mo):   [X]
Users (3 mo):          [X]
Free→paid conversion:  [X%]
Churn target:          [< X%]
```

---

## 🏗️ Development Phases

---

### Phase 0 — MVP
**Goal:** Test the core hypothesis with minimum code
**Timeline:** [2-4 weeks]
**Principle:** Only what the product can't work without at all

#### Included in MVP
- [ ] Authentication (email+password)
- [ ] [Core Feature 1] — [specific, not abstract]
- [ ] [Core Feature 2]
- [ ] [Core Feature 3]
- [ ] Basic UI (functional, not final)

#### NOT included in MVP (consciously)
- [ ] [Feature X] → Phase 1
- [ ] [Feature Y] → Phase 2
- [ ] [Feature Z] → Post-release

#### MVP Done Criteria
```
[ ] User can register
[ ] User can perform the core action (main product action)
[ ] Data is saved and not lost
[ ] Works on mobile browser
[ ] No critical bugs in the happy path
```

#### MVP Success Metric
[What specifically should happen to consider the MVP successful]
Example: "10 users returned on day two" or "first paid conversion"

---

### Phase 1 — Core Product
**Goal:** A product worth using every day
**Timeline:** [1-2 months after MVP]

#### Features
- [ ] [Feature 1] — [detailed description]
  - Sub-feature A
  - Sub-feature B
- [ ] [Feature 2]
- [ ] [Feature 3]
- [ ] Notifications (transactional email)
- [ ] User profile / settings
- [ ] [Product-specific feature]

#### UX Improvements
- [ ] Onboarding flow (how to reach first value)
- [ ] Empty states with character
- [ ] Loading states / skeleton screens
- [ ] Error handling (human-readable messages)
- [ ] Mobile-first responsive

#### Technical Debt from MVP
- [ ] [What needs refactoring after MVP]
- [ ] Tests for core flows
- [ ] Error monitoring (Sentry)

#### Done Criteria
```
[ ] Onboarding: new user reaches value in < 5 minutes
[ ] Retention: 30% of users return within a week
[ ] Performance: LCP < 2.5s
[ ] All main flows covered by tests
```

---

### Phase 2 — Growth Features
**Goal:** Product grows on its own — through sharing, virality, retention
**Timeline:** [2-4 months]

#### Retention & Engagement
- [ ] [Feature that retains users]
- [ ] Email campaigns (onboarding drip, re-engagement)
- [ ] Push/in-app notifications
- [ ] Gamification elements (if appropriate)

#### Monetization (activate)
- [ ] Payment system (Stripe / Lemon Squeezy)
- [ ] Paywall / Feature gating
- [ ] Upgrade flows
- [ ] Billing dashboard

#### Sharing & Virality
- [ ] [Virality mechanism — referral program / public pages / sharing]
- [ ] SEO optimization for public pages (if applicable)
- [ ] OpenGraph tags for sharing

#### Integrations
- [ ] [Integration 1] — [why]
- [ ] [Integration 2]
- [ ] API / Webhooks (if B2B)

#### Done Criteria
```
[ ] First paying users
[ ] MRR > [target]
[ ] Viral coefficient > 0.1 (every 10 users bring 1)
[ ] Churn < X%
```

---

### Phase 3 — Full Release
**Goal:** Mature product ready to scale
**Timeline:** [4-6 months]

#### Scaling
- [ ] Performance optimization under load
- [ ] CDN and caching
- [ ] Database optimization (indexes, query optimization)
- [ ] Horizontal scaling (if needed)

#### Enterprise / Advanced
- [ ] Team accounts / multi-tenancy
- [ ] SSO (if B2B)
- [ ] Advanced analytics for users
- [ ] API for third-party developers (if needed)
- [ ] SLA / Uptime guarantees

#### Quality and Reliability
- [ ] 99.9% uptime SLA
- [ ] Disaster recovery plan
- [ ] GDPR / user data compliance
- [ ] Security audit
- [ ] E2E tests on critical flows

#### Done Criteria
```
[ ] Product handles X concurrent users
[ ] Zero critical bugs for 30 days
[ ] NPS > 40
[ ] User documentation
```

---

## 🔭 Roadmap — Post-Release

> These are not commitments, these are directions.

### Next 6-12 months
- **[Direction 1]:** [What it delivers]
- **[Direction 2]:** [What it delivers]
- **[Direction 3]:** [What it delivers]

### Long-term vision (1-3 years)
[What the product will look like in 2-3 years if everything goes well]

### Ideas backlog (not prioritized)
- [Idea 1]
- [Idea 2]
- [Idea 3]

---

## 🛠 Technical Stack

### Recommended Stack and Rationale

```
Frontend:
  Framework:  [React/Next.js/Vue — WHY]
  Styling:    [Tailwind CSS v4]
  UI Kit:     [shadcn/ui / custom — WHY]
  State:      [Zustand / Redux — WHY]
  Data:       [TanStack Query — WHY]

Backend:
  Runtime:    [Node.js / Python / Go — WHY]
  Framework:  [Express / NestJS / FastAPI — WHY]
  Database:   [PostgreSQL / MongoDB — WHY]
  ORM:        [Prisma / Drizzle / SQLAlchemy — WHY]
  Auth:       [NextAuth / Clerk / custom — WHY]

Infrastructure:
  Hosting:    [Vercel / Railway / AWS — WHY]
  Database:   [Supabase / Neon / PlanetScale — WHY]
  Storage:    [S3 / Cloudflare R2 — WHY]
  Email:      [Resend / SendGrid — WHY]
  Payments:   [Stripe / Lemon Squeezy — WHY]
  Monitoring: [Sentry + Vercel Analytics]

Dev Tools:
  CI/CD:      [GitHub Actions]
  Lint:       [ESLint + Prettier]
  Testing:    [Vitest + Playwright]
```

### Why not [popular alternative]
[If there's a non-obvious choice — explain it]

---

## 🔌 External APIs and Services

| Service | Why | When needed | Price |
|---------|-----|-------------|-------|
| [Service 1] | [Why] | MVP / Phase 1 / 2 | [Free tier / $X/mo] |
| [Service 2] | | | |

---

## 🔐 Security and Compliance

### Minimum from day one
- [ ] HTTPS everywhere
- [ ] Passwords hashed (bcrypt/argon2)
- [ ] SQL injection: parameterized queries / ORM
- [ ] XSS: input sanitization
- [ ] Rate limiting on auth endpoints
- [ ] Environment variables for secrets (not in code)

### Later (Phase 2-3)
- [ ] GDPR: privacy policy, right to deletion
- [ ] Audit logs for sensitive operations
- [ ] Optional 2FA
- [ ] Penetration test before scaling

---

## 📊 Analytics

### What to track from day one
```
Events (Mixpanel / Amplitude / PostHog):
- user_signed_up
- user_completed_onboarding
- [core_action]_completed    ← most important
- subscription_started
- subscription_cancelled

Funnels:
- Registration → First action → Return → Payment

Product Metrics:
- DAU / MAU
- Retention (Day 1, Day 7, Day 30)
- Time to first value
- Feature adoption rate
```

---

## 🗓 Approximate Timeline

```
Week 1-2:    Bootstrap, design system, Auth
Week 3-4:    Core features MVP
Week 5:      QA, bugs, launch preparation
Week 6:      MVP Launch (closed beta)
Month 2-3:   Phase 1 — Core Product
Month 4-5:   Phase 2 — Growth
Month 6+:    Phase 3 — Full Release
```

---

## ❓ Open Questions

> Questions that need to be resolved before or during development

- [ ] [Question 1 — what needs clarification]
- [ ] [Question 2]
- [ ] [Question 3]

---

## 📝 Changelog

| Date | Version | Change | Author |
|------|---------|--------|--------|
| [date] | 1.0 | Initial version | Discovery Agent |

---

> **This document lives alongside the project.**
> When architectural decisions change — update the TZ and create an ADR in `.claude/docs/decisions/`
