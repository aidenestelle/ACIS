---
name: memory-maintenance
description: Prune stale entries, validate memory integrity, archive old data
version: 1.0.0
created: 2025-01-05
times_used: 0
---

# Memory Maintenance Skill

## Purpose

Keep ACIS memory files clean, validated, and performant by pruning stale entries, archiving old data, and ensuring schema integrity.

## Triggers

- Monthly maintenance (recommended)
- Memory files growing too large
- User requests cleanup
- After major project milestones
- When memory_staleness_days threshold exceeded

## Prerequisites

- [ ] Backup memory files before modification (recommended)
- [ ] No active debugging sessions
- [ ] User consent for cleanup

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| mode | string | "review" | "review" (preview), "prune" (execute), "archive" (move old data) |
| staleness_days | number | 30 | From config.json, entries older than this are candidates |
| dry_run | boolean | true | Preview changes without applying |

## Dependencies

None - this is a standalone skill.

## Procedure

### Step 1: Load Configuration

```json
// From config.json
{
  "memory_staleness_days": 30
}
```

### Step 2: Analyze Memory Files

Check each memory file:

#### mistakes.json

```bash
# Count entries
entries: X
debt_observations: Y
inherited_issues: Z
```

**Identify candidates for archival:**
- Entries older than `staleness_days`
- Resolved issues that haven't recurred
- Project-specific items from completed projects

#### knowledge.json

**Review for accuracy:**
- Topology still matches codebase?
- Conventions still valid?
- Patterns.observed ready to crystallize?

#### session-context.json

**Reset stale context:**
- Old pending_tasks that are no longer relevant
- Completed conversation_threads
- Resolved blockers

#### legacy-patterns.json

**Migration cleanup:**
- Completed migrations → move to migration_history
- Obsolete patterns → remove

### Step 3: Generate Report

```markdown
## Memory Maintenance Report
Generated: 2025-01-05

### mistakes.json
- Total entries: 15
- Stale entries (>30 days): 3
- Recommended action: Archive ERR-001, ERR-002, ERR-003

### knowledge.json
- Topology accuracy: Needs review (last updated 45 days ago)
- Patterns ready to crystallize: 2
- Recommended action: Run codebase-discovery to refresh

### session-context.json
- Stale pending_tasks: 5
- Recommended action: Clear completed tasks

### legacy-patterns.json
- Completed migrations: 2
- Recommended action: Move to migration_history
```

### Step 4: Archive Stale Entries (if mode=archive)

Move old entries to archive files:

```
memory/
├── mistakes.json           <- Active entries only
├── knowledge.json
├── session-context.json
├── legacy-patterns.json
└── archive/               <- NEW: archived data
    ├── mistakes-2024.json
    └── mistakes-2025-q1.json
```

Archive format:
```json
{
  "$schema": "Archived ACIS memory entries",
  "archived_date": "2025-01-05",
  "source": "mistakes.json",
  "entries": [
    // Moved entries
  ]
}
```

### Step 5: Validate Schemas (if schemas exist)

If `.claude/schemas/` exists, validate each file:

```bash
# Pseudocode - actual validation
validate(mistakes.json, schemas/mistakes.schema.json)
validate(knowledge.json, schemas/knowledge.schema.json)
validate(session-context.json, schemas/session-context.schema.json)
```

Report validation errors for manual review.

### Step 6: Prune (if mode=prune)

**Only with user consent:**

1. Remove duplicate entries
2. Remove empty arrays/objects
3. Compact whitespace (optional)
4. Update `last_updated` timestamps

### Step 7: Update Session Context

After maintenance:

```json
{
  "last_session": {
    "summary": "Memory maintenance completed",
    "accomplishments": [
      "Archived 5 stale mistake entries",
      "Cleaned 3 pending tasks",
      "Validated all memory schemas"
    ]
  }
}
```

## Validation

- [ ] No data loss (archived, not deleted)
- [ ] All memory files still valid JSON
- [ ] Schema validation passes (if schemas exist)
- [ ] Backup created before modifications
- [ ] Session context updated

## Common Variations

### Quick Review

Just generate report, no changes:
```
mode: "review"
dry_run: true
```

### Full Cleanup

Archive and prune with confirmation:
```
mode: "archive"
dry_run: false
```

### Schema Validation Only

Check integrity without modifications:
```
mode: "validate"
```

## Anti-Patterns

- **Deleting without archiving:** Always preserve history
- **Automated pruning without review:** User should approve removals
- **Ignoring validation errors:** Fix schema issues before they compound
- **Skipping backups:** Always backup before maintenance

## Error Handling

| Error | Solution |
|-------|----------|
| Invalid JSON | Restore from backup, fix manually |
| Schema validation failure | Review and correct data structure |
| Missing archive directory | Create `memory/archive/` |
| Disk space issues | Archive to external location |

## Memory Updates

After this skill completes:

- **session-context.json**: Record maintenance performed
- **All memory files**: Update `last_updated` and `version` as needed

## Related Skills

- `codebase-discovery.md` — If knowledge.json topology is stale
- `debug-session.md` — Source of mistakes.json entries

## Related Mistakes

None directly, but this skill helps prevent mistakes.json from becoming unwieldy.

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-01-05 | Initial skill creation |
