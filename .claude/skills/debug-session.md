---
name: debug-session
description: Structured debugging workflow with automatic post-mortem
version: 1.0.0
created: 2025-01-05
times_used: 0
---

# Debug Session Skill

## Purpose

Systematic approach to debugging with hypothesis testing, isolation, and automatic post-mortem documentation when resolved.

## Triggers

- Debugging session exceeds 15 minutes
- Encountering unexpected behavior
- User reports a bug
- Test failures during development

## Prerequisites

- [ ] Issue is reproducible (or documented steps to trigger)
- [ ] Access to relevant logs/error messages
- [ ] Understanding of expected vs actual behavior

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| issue | string | required | Brief description of the problem |
| severity | string | "medium" | low, medium, high, critical |
| reproduction | string[] | [] | Steps to reproduce |
| start_time | ISO8601 | now | When debugging started |

## Dependencies

None - this is a standalone skill.

## Procedure

### Step 1: Gather Information

Document the issue clearly:

```markdown
## Issue Summary
- **What's happening:** [Actual behavior]
- **What should happen:** [Expected behavior]
- **When it started:** [First noticed / commit / change]
- **Frequency:** [Always / Sometimes / Specific conditions]

## Error Messages
[Copy exact error messages, stack traces]

## Reproduction Steps
1. [Step 1]
2. [Step 2]
3. [Observe error]
```

### Step 2: Check Known Issues

Before deep investigation, check `mistakes.json`:

```bash
# Search for similar patterns
grep -i "<keyword>" memory/mistakes.json
```

Look for:
- Similar symptoms in past entries
- Prevention rules that may apply
- Related tags

### Step 3: Form Hypothesis

Create a hypothesis about the root cause:

```markdown
## Hypothesis 1
**Theory:** [What you think is wrong]
**Evidence:** [Why you think this]
**Test:** [How to verify]
```

### Step 4: Isolate the Problem

Narrow down the scope:

1. **Binary search:** Comment out half the code, does issue persist?
2. **Simplest case:** Create minimal reproduction
3. **Recent changes:** What changed since it last worked?
4. **Dependencies:** Are external services/APIs involved?

```typescript
// Debugging isolation example
console.log('Checkpoint 1: Before suspect function');
const result = suspectFunction(input);
console.log('Checkpoint 2: After suspect function', result);
```

### Step 5: Test Hypothesis

Run your test:

- [ ] If hypothesis confirmed → proceed to fix
- [ ] If hypothesis rejected → form new hypothesis (Step 3)

Track each hypothesis:

```markdown
## Hypothesis Log
| # | Theory | Test | Result |
|---|--------|------|--------|
| 1 | Null user object | Added null check | Rejected - user exists |
| 2 | Race condition in useEffect | Added isMounted flag | **Confirmed** |
```

### Step 6: Implement Fix

Once root cause identified:

1. Write the fix
2. Test the fix
3. Verify no regression
4. Remove debug code (console.logs, etc.)

### Step 7: Post-Mortem

**MANDATORY** after resolving. Add to `mistakes.json`:

```json
{
  "id": "ERR-XXX",
  "timestamp": "2025-01-05T12:00:00Z",
  "category": "logic|syntax|architecture|dependency|convention|security|performance",
  "severity": "low|medium|high|critical",
  "trigger": "What action/pattern triggered this bug",
  "symptom": "What went wrong (observable issue)",
  "root_cause": "Why it happened (actual cause)",
  "resolution": "How it was fixed",
  "prevention_rule": "Before doing X, always check Y",
  "affected_files": ["path/to/file.ts"],
  "tags": ["relevant", "keywords"],
  "debug_duration_minutes": 25,
  "hypotheses_tested": 3
}
```

### Step 8: Update Session Context

Update `session-context.json`:

```json
{
  "last_session": {
    "summary": "Fixed <bug description>",
    "accomplishments": ["Resolved <issue>"],
    "debug_sessions": [{
      "issue": "<brief description>",
      "duration_minutes": 25,
      "resolved": true
    }]
  }
}
```

## Validation

- [ ] Issue is fully resolved
- [ ] No regression introduced
- [ ] Debug artifacts removed (console.logs, debugger statements)
- [ ] Post-mortem added to mistakes.json
- [ ] Prevention rule is actionable

## Common Variations

### Quick Fix (<15 min)

Skip formal post-mortem, but still note if pattern is valuable:
- Add to `patterns.observed` if noteworthy
- Consider full entry if issue could recur

### Inherited Bug (Brownfield)

Use `inherited_issues` instead of `entries`:

```json
{
  "inherited_issues": [{
    "id": "INH-001",
    "description": "Race condition in legacy auth flow",
    "context": "Pre-existing before ACIS",
    "workaround": "Added retry logic",
    "reported_to_user": true
  }]
}
```

### Unresolved After 1 Hour

1. Document current state
2. List hypotheses tested
3. Ask user for guidance
4. Consider escalating or deferring

## Anti-Patterns

- **Shotgun debugging:** Random changes hoping something works
- **Skipping reproduction:** Always reproduce before fixing
- **No post-mortem:** Learn from every significant bug
- **Leaving debug code:** Remove all console.logs, debuggers
- **Fixing symptoms:** Address root cause, not just visible issue

## Error Handling

| Situation | Action |
|-----------|--------|
| Can't reproduce | Get more details, check environment differences |
| Fix breaks other things | Revert, investigate dependencies |
| Third-party bug | Document workaround, report upstream |
| Time limit reached | Document state, ask for guidance |

## Memory Updates

After this skill completes:

- **mistakes.json**: Add post-mortem entry (REQUIRED)
- **session-context.json**: Update with debug session summary
- **knowledge.json**: If architectural insight gained, document it

## Related Skills

- `git-commit.md` — Commit the fix after debugging
- `memory-maintenance.md` — If mistakes.json getting large

## Related Mistakes

Check all entries in `mistakes.json` before starting deep investigation.

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-01-05 | Initial skill creation |
