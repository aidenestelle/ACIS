---
name: git-commit
description: Structured commit workflow with conventional commits and memory updates
version: 1.0.0
created: 2025-01-05
times_used: 0
---

# Git Commit Skill

## Purpose

Execute a complete commit workflow with conventional commit messages, pre-commit validation, and session memory updates.

## Triggers

- User says "commit", "save changes", "commit this"
- After completing a feature or fix
- When explicitly asked to save work

## Prerequisites

- [ ] Changes staged or ready to stage
- [ ] All tests passing (if applicable)
- [ ] Linting passes
- [ ] TypeScript compiles without errors

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| message | string | auto-generated | Commit message (if not provided, generate from changes) |
| scope | string | null | Conventional commit scope (e.g., auth, api, ui) |
| type | string | auto-detect | Commit type: feat, fix, refactor, docs, test, chore |
| breaking | boolean | false | Is this a breaking change? |

## Dependencies

None - this is a standalone skill.

## Procedure

### Step 1: Pre-Commit Validation

Run validation checks before committing:

```bash
# Check for TypeScript errors
npm run typecheck

# Run linting
npm run lint

# Run tests (if quick)
npm run test --run
```

If any fail, fix before continuing.

### Step 2: Review Changes

```bash
# See what will be committed
git status
git diff --staged

# If nothing staged, stage relevant files
git add <files>
```

### Step 3: Determine Commit Type

| Type | When to Use |
|------|-------------|
| `feat` | New feature or functionality |
| `fix` | Bug fix |
| `refactor` | Code restructure without behavior change |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `chore` | Maintenance, dependencies, config |
| `style` | Formatting, whitespace (no logic change) |
| `perf` | Performance improvement |

### Step 4: Craft Commit Message

Follow conventional commit format:

```
<type>(<scope>): <short description>

<body - optional, explain what and why>

<footer - optional, breaking changes, issue refs>
```

**Examples:**
```
feat(auth): add password reset flow

Implements forgot password with email verification.
Adds new edge function for token generation.

Closes #123
```

```
fix(api): handle null response from user endpoint

Previously crashed when user not found. Now returns 404 properly.
```

### Step 5: Commit

```bash
git commit -m "<message>"
```

### Step 6: Update Session Context

After successful commit, update `memory/session-context.json`:

```json
{
  "last_session": {
    "summary": "Added <description>",
    "accomplishments": ["<what was committed>"],
    "last_commit": "<commit hash>"
  }
}
```

## Validation

- [ ] Commit message follows conventional format
- [ ] Type accurately describes the change
- [ ] Scope is relevant (if used)
- [ ] No sensitive data committed (check .env, credentials)
- [ ] Session context updated

## Common Variations

### Quick Commit

For small, obvious changes:
```bash
git add .
git commit -m "fix: typo in UserProfile component"
```

### Amend Last Commit

Only if not pushed yet:
```bash
git add <forgotten-files>
git commit --amend --no-edit
```

### Breaking Change

Include footer:
```
feat(api)!: change response format for /users endpoint

BREAKING CHANGE: Response now returns { data: [...] } instead of raw array.
Migration: Update all consumers to access .data property.
```

## Anti-Patterns

- **Vague messages:** "fixed stuff", "updates", "WIP"
- **Giant commits:** Split into logical chunks
- **Committing .env or secrets:** Always check staged files
- **Skipping tests:** Run at least quick tests before commit
- **Wrong type:** "fix" for features, "feat" for fixes

## Error Handling

| Error | Solution |
|-------|----------|
| Pre-commit hook fails | Fix the issue, re-run `git add` and `git commit` |
| Merge conflict | Resolve conflicts, then commit |
| TypeScript errors | Fix errors before committing |
| Tests failing | Fix tests before committing |

## Memory Updates

After this skill completes:

- **session-context.json**: Update `last_session.summary` and `last_commit`
- **knowledge.json**: If new pattern established, note in `patterns.observed`
- **mistakes.json**: If commit revealed an issue, document it

## Related Skills

- `debug-session.md` — If commit revealed bugs
- `pr-review.md` — After commits, may want to create PR

## Related Mistakes

- ERR-005: Hardcoded secrets (check before commit)

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-01-05 | Initial skill creation |
