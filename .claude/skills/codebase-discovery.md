# Skill: Codebase Discovery

> **Created:** 2025-01-05
> **Last Used:** —
> **Times Used:** 0
> **Version:** 1.0.0

## Purpose

Systematically analyze an existing codebase to understand its structure, patterns, conventions, and technical debt. This skill populates `knowledge.json` and `legacy-patterns.json` before making any changes to a brownfield project.

## Trigger

Conditions that indicate this skill should be used:

- Joining an existing project for the first time
- `knowledge.json` topology is empty but codebase has files
- User asks to "understand the codebase" or "analyze the project"
- Before making significant changes to unfamiliar code
- When conventions are unclear or conflicting

## Prerequisites

- [ ] ACIS system is initialized (`.claude/` directory exists)
- [ ] Project has existing source code to analyze
- [ ] Access to read all source files

## Procedure

### Step 1: Project Identification

Identify the project type and tech stack:

```bash
# Check for package managers and project files
ls -la package.json yarn.lock pnpm-lock.yaml composer.json Gemfile requirements.txt go.mod Cargo.toml pom.xml build.gradle

# Read package.json for JS/TS projects
cat package.json
```

**Record in `knowledge.json` → `stack_detected`:**
- Language(s)
- Framework(s)
- Package manager
- Build tool

### Step 2: Directory Structure Analysis

Map the project structure:

```bash
# Get directory tree (excluding common ignores)
find . -type d -not -path '*/node_modules/*' -not -path '*/.git/*' -not -path '*/dist/*' -not -path '*/.next/*' -not -path '*/build/*' | head -50
```

**Identify and document:**
- Source directories (`src/`, `app/`, `lib/`)
- Test directories (`tests/`, `__tests__/`, `spec/`)
- Configuration location
- Asset directories
- Build output directories

### Step 3: Entry Point Discovery

Find main entry points:

```bash
# Common entry points
cat package.json | grep -E '"main"|"module"|"exports"|"scripts"'

# Check for index files
find . -name 'index.*' -not -path '*/node_modules/*' | head -20

# Check for app/main files
find . -name 'main.*' -o -name 'app.*' -not -path '*/node_modules/*' | head -20
```

**Record in `knowledge.json` → `topology.entry_points`**

### Step 4: Convention Detection

#### 4.1 Naming Conventions

Sample file names to detect patterns:

```bash
# Get sample of source files
find src -type f -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' | head -30
```

**Detect:**
- Components: PascalCase vs kebab-case vs camelCase
- Utilities: naming pattern
- Tests: `.test.ts` vs `.spec.ts` vs `__tests__/`
- Types: `.types.ts` vs `.d.ts` vs inline

#### 4.2 Code Style Analysis

Sample a few representative files:

```typescript
// Look for patterns like:
// - Function components vs class components
// - Named exports vs default exports
// - Type definitions (type vs interface)
// - Import organization
// - Error handling patterns
```

**Record in `knowledge.json` → `conventions`**

### Step 5: Architectural Pattern Detection

Identify the architecture:

| Pattern | Indicators |
|---------|------------|
| Feature-based | `features/`, `modules/` directories with self-contained code |
| Layer-based | `controllers/`, `services/`, `repositories/` separation |
| Component-based | `components/` with flat or nested structure |
| MVC | `models/`, `views/`, `controllers/` |
| Domain-driven | `domain/`, `application/`, `infrastructure/` |

**Record in `knowledge.json` → `topology.architecture_pattern`**

### Step 6: Dependency Analysis

Review key dependencies:

```bash
# For JS/TS projects
cat package.json | jq '.dependencies, .devDependencies'
```

**Categorize:**
- Core framework dependencies
- State management
- API/data fetching
- UI component libraries
- Testing tools
- Build tools

**Record critical dependencies in `knowledge.json` → `dependencies.critical`**

### Step 7: Technical Debt Identification

Look for signs of technical debt:

```bash
# Find TODO/FIXME comments
grep -r "TODO\|FIXME\|HACK\|XXX" --include="*.ts" --include="*.tsx" --include="*.js" | head -20

# Find any files
grep -r "any" --include="*.ts" --include="*.tsx" | wc -l

# Look for inconsistent patterns
find . -name '*.js' -path '*/src/*' | wc -l  # JS in TS project?
```

**Document in `legacy-patterns.json` → `technical_debt`**

### Step 8: Pattern Divergence Detection

Find where conventions aren't followed:

- Mixed naming conventions
- Inconsistent file organization
- Multiple ways of doing the same thing
- Deprecated patterns still in use

**Record in `legacy-patterns.json`:**
- `preserve`: Patterns that are intentional and should continue
- `migrate_when_touched`: Patterns to update opportunistically
- `technical_debt`: Issues to address when prioritized

### Step 9: Generate Summary

Create a discovery summary:

```markdown
## Codebase Discovery Summary

**Project:** [Name]
**Stack:** [Detected technologies]
**Architecture:** [Pattern identified]

### Key Findings
- [Finding 1]
- [Finding 2]

### Conventions Detected
- Components: [Convention]
- Files: [Convention]
- Exports: [Convention]

### Technical Debt Noted
- [ ] [Debt item 1]
- [ ] [Debt item 2]

### Recommendations
- [Recommendation 1]
- [Recommendation 2]
```

### Step 10: Update Memory Files

Populate all relevant memory files:

1. **`knowledge.json`**: Full topology and conventions
2. **`legacy-patterns.json`**: Patterns to preserve/migrate
3. **`session-context.json`**: Note discovery was completed
4. **`config.json`**: Update stack_hints if empty

## Validation

How to confirm the skill executed successfully:

- [ ] `knowledge.json` → `topology` is populated with entry points and modules
- [ ] `knowledge.json` → `conventions` reflects actual codebase patterns
- [ ] `legacy-patterns.json` exists with at least one entry
- [ ] Stack hints in `config.json` match detected technologies
- [ ] Discovery summary has been presented to user

## Common Variations

| Variation | Description | Modified Steps |
|-----------|-------------|----------------|
| Monorepo | Project contains multiple packages | Add Step 2.5: Package enumeration |
| Polyglot | Multiple languages in use | Repeat Steps 4-5 per language |
| No package.json | Non-JS project | Adapt Step 1 for project type |
| Massive codebase | >1000 files | Sample instead of full scan |

## Anti-Patterns

What NOT to do when using this skill:

- **Don't:** Fix issues during discovery — Note them, don't change them
- **Don't:** Assume conventions from one area apply everywhere — Verify consistency
- **Don't:** Skip dependency analysis — It reveals architectural decisions
- **Don't:** Ignore test patterns — They indicate quality expectations
- **Don't:** Rush the process — Thorough discovery prevents mistakes later

## Error Handling

| Error | Likely Cause | Solution |
|-------|--------------|----------|
| Can't determine framework | Non-standard setup | Check for custom build scripts |
| Conflicting conventions | Evolved over time | Document both, note which to prefer |
| Missing entry points | Unconventional structure | Look for build config, ask user |
| Too many patterns | Large/old codebase | Focus on actively maintained areas |

## Related Skills

- [`_template.md`](./_template.md) — Skill template

## Related Mistakes

Reference to `memory/mistakes.json` entries that are relevant:

- (None yet — this skill helps prevent discovery-related mistakes)

## Changelog

| Date | Version | Changes |
|------|---------|---------|
| 2025-01-05 | 1.0.0 | Initial creation |

## Notes

- This skill should be run early in any brownfield engagement
- Results should be reviewed with the user to confirm accuracy
- Discovery depth can be adjusted based on project size and time constraints
- Re-run periodically on large projects to catch drift
