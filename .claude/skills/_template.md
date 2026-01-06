---
name: [skill-name]
description: [One-line description of what this skill does]
version: 1.0.0
created: [ISO8601 timestamp]
times_used: 0
---

# [Skill Name]

## Purpose

[One paragraph explaining what this skill accomplishes and why it exists.]

## Triggers

Conditions that indicate this skill should be used:

- [Condition 1 that should trigger this skill]
- [Condition 2]
- [Explicit user request pattern, e.g., "when user asks to..."]

## Prerequisites

- [ ] [Prerequisite 1 - e.g., "Node.js installed"]
- [ ] [Prerequisite 2 - e.g., "package.json exists"]
- [ ] [Prerequisite 3 - e.g., "Required environment variables set"]

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| [param1] | string | required | [Description of required parameter] |
| [param2] | string | null | [Description of optional parameter] |
| [param3] | boolean | false | [Description of flag parameter] |

## Dependencies

Skills that must run before this one:

- `skill-name.md` — [Why it's required]
- None (if standalone)

## Procedure

### Step 1: [Action Name]

[Detailed instructions for step 1]

```[language]
[Code example if applicable]
```

### Step 2: [Action Name]

[Detailed instructions for step 2]

### Step 3: [Action Name]

[Detailed instructions for step 3]

## Validation

How to confirm the skill executed successfully:

- [ ] [Verification check 1]
- [ ] [Verification check 2]
- [ ] [Verification check 3]

## Common Variations

| Variation | Description | Modified Steps |
|-----------|-------------|----------------|
| [Variation A] | [When/why to use this variation] | [Which steps change] |
| [Variation B] | [When/why to use this variation] | [Which steps change] |

## Anti-Patterns

What NOT to do when using this skill:

- **Don't:** [Anti-pattern 1] — [Why it's bad]
- **Don't:** [Anti-pattern 2] — [Why it's bad]
- **Don't:** [Anti-pattern 3] — [Why it's bad]

## Error Handling

| Error | Likely Cause | Solution |
|-------|--------------|----------|
| [Error message/symptom] | [What causes it] | [How to fix] |
| [Error message/symptom] | [What causes it] | [How to fix] |

## Memory Updates

After this skill completes, update these files:

- **mistakes.json**: [When to add entries, what to record]
- **knowledge.json**: [When to update topology, conventions, patterns]
- **session-context.json**: [What context to preserve]

## Related Skills

- [`other-skill.md`](./other-skill.md) — [Why it's related]
- [`another-skill.md`](./another-skill.md) — [Why it's related]

## Related Mistakes

Reference to `memory/mistakes.json` entries that are relevant:

- `ERR-XXX` — [Brief description]

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | [ISO8601] | Initial creation |

## Notes

[Any additional context, edge cases, performance considerations, or warnings.]
