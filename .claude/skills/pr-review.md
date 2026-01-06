---
name: pr-review
description: Code review checklist and process for pull requests
version: 1.0.0
created: 2025-01-05
times_used: 0
---

# PR Review Skill

## Purpose

Systematic code review process for pull requests, ensuring quality, consistency, and catching issues before merge.

## Triggers

- User asks to "review PR", "check changes", "review code"
- Before merging feature branches
- When reviewing external contributions

## Prerequisites

- [ ] PR/changes available for review
- [ ] Understanding of what the changes should accomplish
- [ ] Access to diff and context

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| pr_url | string | null | GitHub/GitLab PR URL (optional) |
| focus | string[] | ["all"] | Specific focus areas: code, security, performance, tests |
| depth | string | "standard" | "quick" (15 min), "standard" (30 min), "thorough" (60 min) |

## Dependencies

None - this is a standalone skill.

## Procedure

### Step 1: Understand Context

Before reviewing code:

1. **Read the PR description** - What problem does this solve?
2. **Check linked issues** - What was the original request?
3. **Review commit history** - How did the solution evolve?

```markdown
## PR Context
- **Purpose:** [What this PR accomplishes]
- **Related Issue:** #[number]
- **Type:** feat | fix | refactor | docs | test | chore
```

### Step 2: High-Level Review

Get the big picture first:

- [ ] Does the approach make sense?
- [ ] Is the scope appropriate (not too big)?
- [ ] Are there architectural concerns?
- [ ] Does it align with project conventions?

### Step 3: Detailed Code Review

#### Code Quality Checklist

| Check | Status | Notes |
|-------|--------|-------|
| **Readability** | | |
| Clear variable/function names | | |
| Appropriate comments (not excessive) | | |
| Logical file organization | | |
| **Correctness** | | |
| Logic handles edge cases | | |
| Error handling present | | |
| No obvious bugs | | |
| **Conventions** | | |
| Follows TypeScript conventions | | |
| Follows React conventions | | |
| Follows project naming patterns | | |
| **Performance** | | |
| No unnecessary re-renders | | |
| Appropriate memoization | | |
| Efficient algorithms | | |

#### Security Checklist

| Check | Status | Notes |
|-------|--------|-------|
| No hardcoded secrets | | |
| Input validation present | | |
| Proper authentication/authorization | | |
| No SQL injection vectors | | |
| No XSS vulnerabilities | | |

#### Test Checklist

| Check | Status | Notes |
|-------|--------|-------|
| Tests added for new functionality | | |
| Tests cover happy path | | |
| Tests cover edge cases | | |
| Tests cover error cases | | |
| Existing tests still pass | | |

### Step 4: Check Against ACIS Standards

Reference ACIS conventions:

```markdown
## ACIS Compliance
- [ ] Pre-flight check would pass for these changes
- [ ] No patterns in mistakes.json violated
- [ ] Matches conventions in knowledge.json
- [ ] Within thresholds (max_lines_per_file_change: 300)
```

### Step 5: Identify Issues

Categorize findings:

```markdown
## Issues Found

### Blocking (Must Fix)
- [ ] Issue 1: [Description] - [File:Line]

### Significant (Should Fix)
- [ ] Issue 2: [Description] - [File:Line]

### Minor (Nice to Fix)
- [ ] Issue 3: [Description] - [File:Line]

### Suggestions (Optional)
- [ ] Suggestion 1: [Description]
```

### Step 6: Provide Feedback

Structure feedback constructively:

**For issues:**
```markdown
**File:** src/components/UserProfile.tsx:42
**Issue:** Missing error handling for API call
**Suggestion:** Wrap in try/catch and show error state
**Example:**
\`\`\`typescript
try {
  const data = await fetchUser(id);
} catch (error) {
  setError(error.message);
}
\`\`\`
```

**For praise:**
```markdown
**File:** src/hooks/useAuth.ts
**Note:** Great use of AbortController for cleanup!
```

### Step 7: Render Verdict

```markdown
## Review Summary

**Verdict:** Approved | Approved with Suggestions | Changes Requested | Blocked

**Summary:**
[Brief overall assessment]

**Blocking Issues:** [count]
**Significant Issues:** [count]
**Minor Issues:** [count]

**Next Steps:**
- [What author should do]
```

### Step 8: Update Memory (if patterns found)

If review reveals reusable insights:

**Add to knowledge.json:**
```json
{
  "patterns": {
    "observed": [
      "API error handling pattern emerged in multiple PRs"
    ]
  }
}
```

**Add to mistakes.json (if recurring issues):**
```json
{
  "entries": [{
    "trigger": "PR review caught missing error handling",
    "prevention_rule": "All API calls must have error handling"
  }]
}
```

## Validation

- [ ] All files reviewed
- [ ] Checklist completed
- [ ] Feedback is constructive
- [ ] Verdict is clear
- [ ] Next steps defined

## Common Variations

### Quick Review

Focus on critical issues only:
- Security vulnerabilities
- Breaking changes
- Obvious bugs

### Thorough Review

Include:
- Performance analysis
- Test coverage assessment
- Documentation review
- Accessibility check

### Self-Review

Before submitting your own PR:
- Use same checklist
- Extra focus on blind spots
- Ask: "Would I approve this?"

## Anti-Patterns

- **Nitpicking:** Focus on substance over style
- **Vague feedback:** Be specific with file/line references
- **No examples:** Show how to fix, not just what's wrong
- **Rubber stamping:** Actually review the code
- **Blocking over preferences:** Block only for real issues

## Error Handling

| Situation | Action |
|-----------|--------|
| PR too large | Request split into smaller PRs |
| Missing context | Ask author for explanation |
| Unfamiliar code | Ask questions, don't assume |
| Conflict with author | Focus on facts, escalate if needed |

## Memory Updates

After this skill completes:

- **session-context.json**: Record review performed
- **knowledge.json**: Add observed patterns
- **mistakes.json**: Add recurring issues found

## Related Skills

- `git-commit.md` — After author addresses feedback
- `debug-session.md` — If bugs found during review

## Related Mistakes

- ERR-001 through ERR-010: Check all common mistakes during review

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-01-05 | Initial skill creation |
