# ACIS — Adaptive Codebase Intelligence System

> Persistent memory and workflow system for Claude Code

**Version:** 1.2.0 | **Updated:** 2025-01-05

---

## Quick Start

1. Load `config.json` for settings
2. Load `memory/session-context.json` for previous context
3. Check `memory/mistakes.json` for active warnings
4. Reference `memory/knowledge.json` for conventions

**Brownfield?** Run `skills/codebase-discovery.md` first if `knowledge.json` is empty.

---

## Critical Rules (NEVER Violate)

1. **Pre-flight check before ANY file mutation** — see below
2. **Never use `any`** — use `unknown` with type guards
3. **Update memory after significant work**
4. **Check mistakes.json before similar code**
5. **No hardcoded secrets** — use environment variables

---

## System Structure

```
.claude/
├── CLAUDE.md              <- You are here
├── config.json            <- Settings and thresholds
├── conventions/           <- Stack-specific conventions
│   ├── typescript.md, react.md, supabase.md, tailwind.md
├── memory/
│   ├── mistakes.json      <- Error prevention
│   ├── knowledge.json     <- Codebase intelligence
│   ├── session-context.json, legacy-patterns.json
├── skills/                <- Crystallized workflows
├── agents/                <- Planner, Executor, Reviewer
├── reference/             <- Emergency procedures
├── schemas/               <- JSON validation
└── logs/                  <- Telemetry
```

---

## Core Workflow

```
User Request → PLANNER → EXECUTOR → REVIEWER → Memory Update
```

| Phase | Agent | Purpose |
|-------|-------|---------|
| Plan | `agents/planner.md` | Decompose, check history, structure tasks |
| Execute | `agents/executor.md` | Pre-flight, implement, document |
| Review | `agents/reviewer.md` | Validate, approve/reject |

**Skip for trivial tasks** (see `config.json` → `complexity_definitions`)

---

## Pre-Flight Check Protocol

**MANDATORY before ANY file write/edit:**

```
[ ] Checked mistakes.json for similar past errors
[ ] Change aligns with knowledge.json decisions
[ ] Following conventions (see conventions/)
[ ] Within config.json thresholds (10 files, 300 lines)
[ ] Pattern done 3+ times? Propose skill
```

**Brownfield additions:**
```
[ ] Checked legacy-patterns.json for preserved patterns
[ ] Matching existing style in file/module
[ ] Not doing "drive-by refactoring"
```

---

## Post-Mortem Protocol

**After ANY failure or debugging >15 minutes:**

1. **WHAT** — symptom
2. **WHY** — root cause (ask "why" 5×)
3. **HOW** — resolution
4. **PREVENT** — actionable rule

→ Add to `memory/mistakes.json`

---

## Memory System

| File | Purpose | Update When |
|------|---------|-------------|
| `mistakes.json` | Error prevention | After bugs, debugging, reviews |
| `knowledge.json` | Codebase map | New modules, decisions, conventions |
| `session-context.json` | Session state | End of session, branch switch |
| `legacy-patterns.json` | Brownfield patterns | Discovery, migrations |

---

## Conventions

See `conventions/` for detailed patterns:

| Topic | File | Key Points |
|-------|------|------------|
| TypeScript | `typescript.md` | `type` over `interface`, no `any`, explicit returns |
| React | `react.md` | Cleanup useEffect, stable keys, no mutations |
| Supabase | `supabase.md` | Always handle errors, use generated types |
| Tailwind | `tailwind.md` | Class order: layout → spacing → colors → effects |

---

## Skills

Available in `skills/`:

| Skill | Purpose |
|-------|---------|
| `codebase-discovery.md` | Analyze brownfield projects |
| `git-commit.md` | Commit workflow |
| `debug-session.md` | Structured debugging with post-mortem |
| `memory-maintenance.md` | Prune/archive memory |
| `pr-review.md` | Code review process |

**Create new skill** when pattern repeats 3+ times.

---

## File Naming

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase + `use` | `useAuth.ts` |
| Utilities | camelCase | `formatDate.ts` |
| Types | PascalCase + `.types.ts` | `User.types.ts` |
| Tests | + `.test.ts` | `UserProfile.test.tsx` |

---

## Configuration

From `config.json`:

| Setting | Default | Purpose |
|---------|---------|---------|
| `max_file_changes_per_commit` | 10 | Keep commits focused |
| `max_lines_per_file_change` | 300 | Keep changes reviewable |
| `pattern_recognition_min_occurrences` | 3 | When to propose skills |
| `memory_staleness_days` | 30 | When to review entries |

---

## Graceful Degradation

| Missing | Behavior |
|---------|----------|
| `config.json` | Use defaults (10 files, 300 lines) |
| `mistakes.json` | No historical prevention |
| `knowledge.json` | Discover from codebase |
| `session-context.json` | Fresh start |
| `agents/*.md` | Inline reasoning |

---

## Quick Commands

```bash
npm run dev          # Dev server
npm run build        # Production build
npm run typecheck    # TypeScript check
npm run test         # Run tests
npm run lint         # Lint check
npm run gen:types    # Supabase types
```

---

## Emergency Procedures

See `reference/emergency-procedures.md` for build failures, Supabase errors, test failures.

---

## Anti-Patterns

- **Skipping pre-flight** — Never skip
- **Using `any`** — Use `unknown` + type guards
- **Silent error swallowing** — Always handle
- **Index as React key** — Use stable IDs
- **Direct state mutation** — Use setState
- **Hardcoded secrets** — Use env vars
- **Missing useEffect cleanup** — Always cleanup

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.2.0 | 2025-01-05 | Optimization: reduced redundancy, added skills, telemetry |
| 1.1.0 | 2025-01-05 | Added brownfield support |
| 1.0.0 | 2025-01-03 | Initial implementation |

---

*This file is the kernel. Modifications change system behavior.*
