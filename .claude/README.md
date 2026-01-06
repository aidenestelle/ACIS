# ACIS — Adaptive Codebase Intelligence System

A persistent memory and workflow system for [Claude Code](https://claude.ai/claude-code) that learns, remembers, and improves over time.

## What is ACIS?

ACIS transforms Claude Code from a stateless assistant into an intelligent partner that:

- **Remembers past mistakes** and proactively avoids them
- **Understands your codebase** structure, patterns, and conventions
- **Maintains session continuity** across conversations
- **Crystallizes repeated workflows** into reusable skills
- **Follows a structured workflow** (Plan → Execute → Review)

## Quick Start

### Greenfield Projects (Starting Fresh)

1. Copy the `.claude/` directory to your project root
2. Update `config.json` with your stack preferences
3. Claude Code will automatically read `CLAUDE.md` and follow the system

### Brownfield Projects (Existing Codebases)

1. Copy the `.claude/` directory to your project root
2. Clear or leave `config.json` stack_hints empty (ACIS will auto-detect)
3. On first use, ACIS will run **Codebase Discovery** before making changes
4. Review the populated `memory/knowledge.json` and `memory/legacy-patterns.json`

ACIS respects existing patterns and avoids "drive-by refactoring" in brownfield projects.

## Directory Structure

```
.claude/
├── CLAUDE.md                 # The kernel — core instructions
├── config.json               # Settings, thresholds, agent configuration
├── README.md                 # You are here
├── conventions/              # Stack-specific conventions (NEW in v1.2.0)
│   ├── typescript.md         # TypeScript patterns
│   ├── react.md              # React hooks and components
│   ├── supabase.md           # Supabase queries and auth
│   └── tailwind.md           # Tailwind CSS organization
├── memory/
│   ├── mistakes.json         # Error prevention database (pre-populated)
│   ├── knowledge.json        # Codebase intelligence & decisions
│   ├── legacy-patterns.json  # Patterns to preserve/migrate (brownfield)
│   └── session-context.json  # Session continuity
├── skills/
│   ├── _template.md          # Template for creating new skills
│   ├── codebase-discovery.md # Analyze existing codebases
│   ├── git-commit.md         # Commit workflow (NEW in v1.2.0)
│   ├── debug-session.md      # Structured debugging (NEW in v1.2.0)
│   ├── memory-maintenance.md # Memory cleanup (NEW in v1.2.0)
│   └── pr-review.md          # Code review (NEW in v1.2.0)
├── agents/
│   ├── planner.md            # Task decomposition agent
│   ├── executor.md           # Implementation agent
│   └── reviewer.md           # Quality assurance agent
├── reference/                # Reference materials (NEW in v1.2.0)
│   └── emergency-procedures.md
├── schemas/                  # JSON validation schemas (NEW in v1.2.0)
│   ├── mistakes.schema.json
│   ├── knowledge.schema.json
│   └── session-context.schema.json
└── logs/
    └── telemetry.json        # Usage analytics (NEW in v1.2.0)
```

## Core Components

### CLAUDE.md — The Kernel (~220 lines)

Streamlined instruction file containing:
- Critical rules and pre-flight checks
- Core workflow overview
- Memory system reference
- Anti-patterns to avoid

Detailed conventions are now in `conventions/` directory.

### config.json — Configuration

Now includes agent configuration and complexity definitions:

```json
{
  "complexity_definitions": {
    "trivial": "single_file_under_20_lines",
    "simple": "single_file_any_size",
    "moderate": "2_to_5_files",
    "complex": "more_than_5_files_or_arch_impact"
  },
  "agents": {
    "planner": { "skip_for_complexity": ["trivial"] },
    "reviewer": { "skip_for_complexity": ["trivial"] }
  }
}
```

### Conventions Directory (NEW in v1.2.0)

Stack-specific patterns extracted from CLAUDE.md for better organization:

| File | Content |
|------|---------|
| `typescript.md` | Type definitions, `unknown` vs `any`, generics |
| `react.md` | Hooks rules, state management, event handlers |
| `supabase.md` | Query patterns, auth, real-time, error handling |
| `tailwind.md` | Class organization, responsive, accessibility |

### Skills (Expanded in v1.2.0)

| Skill | Purpose |
|-------|---------|
| `codebase-discovery.md` | Analyze brownfield projects |
| `git-commit.md` | Conventional commits workflow |
| `debug-session.md` | Structured debugging with post-mortem |
| `memory-maintenance.md` | Archive stale entries, validate schemas |
| `pr-review.md` | Code review checklist and process |

## Core Workflow

```
User Request → PLANNER → EXECUTOR → REVIEWER → Memory Update
```

**Skip agents for trivial tasks** based on `complexity_definitions` in config.

## Memory System

| File | Purpose | Pre-populated |
|------|---------|---------------|
| `mistakes.json` | Error prevention | Yes (10 common React/TS errors) |
| `knowledge.json` | Codebase map | Schema only |
| `session-context.json` | Session state | Schema only |
| `legacy-patterns.json` | Brownfield patterns | Schema only |

### Pre-Populated Mistakes

v1.2.0 includes 10 common mistakes:
- ERR-001: Using `any` type
- ERR-002: Missing useEffect cleanup
- ERR-003: Array index as React key
- ERR-004: Direct state mutation
- ERR-005: Hardcoded secrets
- ERR-006: Missing Supabase error handling
- ERR-007: Implicit any in event handlers
- ERR-008: Missing useEffect dependencies
- ERR-009: Async useEffect without cleanup
- ERR-010: Prop drilling

## What's New in v1.2.0

### Reduced Redundancy
- CLAUDE.md slimmed from ~580 to ~220 lines
- Stack conventions moved to `conventions/` directory
- Agents reference shared conventions instead of duplicating

### Expanded Skills
- 4 new skills: git-commit, debug-session, memory-maintenance, pr-review
- Enhanced skill template with parameters and dependencies

### Enhanced Configuration
- Agent configuration for skip conditions
- Complexity definitions for workflow decisions
- Telemetry setting

### Added Infrastructure
- `conventions/` directory with 4 stack-specific files
- `reference/emergency-procedures.md` for troubleshooting
- `schemas/` directory with JSON Schema validation
- `logs/telemetry.json` for usage tracking

### Fixed Issues
- Type/interface inconsistency (now consistently uses `type`)
- Version mismatch in CLAUDE.md

## Customization

### Adapting for Your Stack

1. **Update conventions files** in `conventions/`
2. **Modify `config.json`** stack_hints
3. **Pre-populate `knowledge.json`** with your architecture

### Adding Custom Skills

1. Copy `skills/_template.md` to `skills/your-skill.md`
2. Fill in all sections including Parameters and Dependencies
3. Reference in `knowledge.json` → `patterns.crystallized_as_skills`

## Best Practices

1. **Commit `.claude/` to your repository** — Share knowledge across the team
2. **Review memory files periodically** — Use `memory-maintenance.md` skill monthly
3. **Let skills crystallize naturally** — Don't force them; let patterns emerge
4. **Trust the pre-flight check** — Never skip it, even for "quick fixes"
5. **Run post-mortems consistently** — Every debugging session is a learning opportunity

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.2.0 | 2025-01-05 | Reduced redundancy, added skills, conventions, telemetry |
| 1.1.0 | 2025-01-05 | Added brownfield project support |
| 1.0.0 | 2025-01-03 | Initial ACIS implementation |

## License

This configuration system is provided as-is for use with Claude Code. Modify freely for your projects.

---

*ACIS learns from every session. The more you use it, the smarter it gets.*
