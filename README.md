# claude-forge 🔥

**From idea to project infrastructure — in one command.**

claude-forge is a starter kit for working with Claude Code.
Drop the files into an empty project, say `bootstrap` — and get
a complete infrastructure: agents, skills, documentation, structure.

Have a spec? Claude reads it and scaffolds the project accordingly.
No spec? The Discovery Agent listens to your idea, does research,
asks the right questions, and creates the spec for you.

---

## What's Inside

```
claude-forge/
├── BOOTSTRAP-v2.md          ← entry point, main protocol
└── .claude/
    └── skills/
        ├── idea-discovery.md    ← Discovery Agent (no spec → creates spec)
        ├── TZ-template.md       ← spec format: MVP + phases + roadmap
        ├── frontend-design.md   ← aesthetic direction, anti-template
        ├── design-tokens.md     ← palette, fonts, spacing
        ├── typography.md        ← hierarchy, font pairing, details
        ├── layout-craft.md      ← composition, hierarchy, breathing room
        ├── motion.md            ← framer-motion patterns, micro-interactions
        └── ux-states.md         ← loading / empty / error states
```

---

## Quick Start

### 1. Clone into an empty project

```bash
# Option A — if the project already exists:
cd my-project
git clone https://github.com/EdMinor/claude-forge .claude-forge-tmp
cp .claude-forge-tmp/BOOTSTRAP-v2.md .
cp -r .claude-forge-tmp/.claude .
rm -rf .claude-forge-tmp

# Option B — use as a GitHub template
# Click "Use this template" → Create new repository
```

### 2. Optionally: add a spec

If you already have a technical specification — create `TZ.md` in the project root.
No spec? Not needed — the Discovery Agent will help you create one.

### 3. Launch Claude Code

```bash
claude
```

### 4. One command

```
bootstrap
```

That's it. Claude reads `BOOTSTRAP-v2.md`, checks for `TZ.md`,
and follows the appropriate path.

---

## Two Paths

### Path A — TZ.md exists
```
bootstrap → reads TZ → determines stack and modules
         → creates .claude/CLAUDE.md
         → generates project-specific skills
         → creates agents (orchestrator + per module)
         → sets up documentation
         → initializes src/ structure
         → done
```

### Path B — no TZ.md
```
bootstrap → launches Discovery Agent
         → listens to the idea (free form)
         → researches the niche and competitors
         → asks questions in blocks with choice options
         → fills gaps with domain expertise
         → shows summary → gets confirmation
         → generates TZ.md (MVP + 3 phases + roadmap + stack)
         → → Path A
```

---

## What Gets Auto-Generated

After `bootstrap`, the project gets:

**Agent Infrastructure**
```
.claude/
├── CLAUDE.md                    ← instructions for all agents
├── skills/                      ← project-specific skills
└── agents/
    ├── orchestrator.md          ← master agent
    ├── [module]-agent.md        ← one per module
    └── qa-agent.md
```

**Living Documentation**
```
.claude/docs/
├── context/current-context.md  ← cross-session memory
├── daily/                      ← daily logs
├── decisions/                  ← ADR (Architecture Decision Records)
├── errors/solved/              ← solved error encyclopedia
├── weekly/                     ← weekly reports
└── milestones/                 ← project milestones
```

**Project Structure**
```
src/
├── features/      ← one folder per TZ module
├── components/ui/ ← shadcn + base components
├── design-system/ ← tokens.ts + theme.css
├── api/           ← API client and endpoints
└── ...
```

---

## Design Skills — Application Order

One of the core principles of claude-forge — a project should never look
like an out-of-the-box template. Skills are applied in a specific order:

```
Before first component:
  1. design-tokens.md    → custom palette, not default blue
  2. typography.md       → font pairing, hierarchy
  3. frontend-design.md  → aesthetic direction

Per screen:
  4. layout-craft.md     → composition, hierarchy, breathing room
  5. ux-states.md        → loading / empty / error (not a spinner!)

After base structure:
  6. motion.md           → page transitions, micro-interactions
```

---

## Spec Format

`TZ-template.md` contains the structure that the Discovery Agent generates:

- **Vision** — essence, problem, solution, UVP
- **Audience** — personas, who is NOT the audience
- **Market Context** — competitors, positioning
- **Monetization** — model, pricing, metrics
- **Phase 0 — MVP** — minimum to test the hypothesis
- **Phase 1 — Core Product** — a product worth using daily
- **Phase 2 — Growth** — retention, monetization, virality
- **Phase 3 — Full Release** — scaling, enterprise
- **Roadmap** — post-release, long-term vision
- **Stack** — with rationale for every choice
- **Analytics** — what to track from day one

---

## Project Documentation (Living System)

claude-forge launches a documentation system that lives alongside the project:

| Document | When Created |
|----------|-------------|
| `current-context.md` | Updated every session — cross-chat memory |
| `daily/YYYY-MM-DD.md` | Every work session |
| `decisions/ADR-NNN.md` | On architectural decisions |
| `errors/solved/ERR-NNN.md` | After solving a non-trivial bug |
| `weekly/week-XX.md` | Once a week |
| `milestones/M-N.md` | When a milestone is closed |

---

## Skill Evolution

Skills are not static — they get updated as the project grows.
After every completed module, the agent updates relevant skills:
adds discovered patterns, documents anti-patterns, bumps the version.

```markdown
## Evolution log
- v1.0: initial version from bootstrap
- v1.2: added patterns after Dashboard module
- v1.3: documented pitfalls from UserProfile
```

---

## Compatibility

- **Claude Code** — primary environment, everything is built for it
- **Claude Sonnet 4+** — recommended model
- **Any frontend stack** — skills adapt to the project
- **Fullstack projects** — supported, agents split by module

---

## Author

Created by [Eduard Minor](https://github.com/EdMinor) in collaboration with Claude.

*"Coding is both art and logic — sometimes messy like a painter's studio,
but always driven by curiosity and inspiration."*

---

## License

MIT — use, fork, improve.
