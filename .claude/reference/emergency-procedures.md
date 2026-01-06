# Emergency Procedures

> Quick reference for common ACIS emergencies and troubleshooting

---

## Build Failures

### TypeScript Errors

```bash
npm run typecheck
```

**Common causes:**
1. Missing type definitions
2. Incorrect import paths
3. Type mismatches after refactoring

**Solutions:**
- Check the exact error location
- Verify import paths are correct
- Update types to match actual data structures
- Run `npm install` if missing @types packages

### Circular Dependencies

**Symptoms:**
- Unexpected `undefined` imports
- Module not found errors during runtime

**Solutions:**
1. Use dependency analysis: `npx madge --circular src/`
2. Refactor to break the cycle:
   - Extract shared code to a new module
   - Use dependency injection
   - Lazy import with dynamic `import()`

### Missing Dependencies

```bash
npm install
```

If specific package missing:
```bash
npm install <package-name>
```

---

## Supabase Errors

### RLS Policy Issues

**Symptoms:**
- Queries return empty when data exists
- "new row violates row-level security policy" errors

**Solutions:**
1. Check RLS policies in Supabase Dashboard
2. Verify user is authenticated
3. Check policy conditions match the query
4. Temporarily disable RLS to confirm it's the issue

### Type Mismatches

```bash
npm run gen:types
```

**Symptoms:**
- TypeScript errors after schema changes
- Runtime errors accessing missing columns

**Solutions:**
1. Regenerate types after schema changes
2. Update all usages of changed tables
3. Clear TypeScript cache: `rm -rf node_modules/.cache`

### Environment Variables

**Symptoms:**
- "Missing Supabase URL" errors
- Connection failures

**Solutions:**
1. Verify `.env` file exists
2. Check variable names match expected format:
   ```
   VITE_SUPABASE_URL=https://xxx.supabase.co
   VITE_SUPABASE_ANON_KEY=xxx
   ```
3. Restart dev server after changes

### Service Status

Check Supabase dashboard: https://supabase.com/dashboard

---

## Test Failures

### Outdated Tests

**Symptoms:**
- Tests fail after code changes
- Mock data doesn't match types

**Solutions:**
1. Update test expectations to match new behavior
2. Update mock data to match current types
3. Add new tests for new functionality

### Async Timing Issues

**Symptoms:**
- Tests pass/fail inconsistently
- "act()" warnings in React tests

**Solutions:**
1. Use `await waitFor()` for async operations
2. Use `act()` wrapper for state updates
3. Increase timeout if needed: `jest.setTimeout(10000)`

### Isolation Testing

Run single test to debug:
```bash
npm run test -- --run path/to/test.test.ts
```

---

## Memory File Corruption

### Invalid JSON

**Symptoms:**
- ACIS fails to load
- JSON parse errors

**Solutions:**
1. Check file for syntax errors (trailing commas, unclosed brackets)
2. Use JSON validator
3. Restore from git: `git checkout memory/file.json`

### Schema Validation Failures

If schemas exist in `.claude/schemas/`:
1. Validate file against schema
2. Fix structural issues
3. Run `skills/memory-maintenance.md` to repair

---

## Git Issues

### Merge Conflicts in Memory Files

**Solutions:**
1. For `session-context.json`: Keep current session data
2. For `mistakes.json`: Merge entries from both branches
3. For `knowledge.json`: Carefully merge, prefer newer conventions

### Accidental Commits

**Undo last commit (not pushed):**
```bash
git reset --soft HEAD~1
```

**Undo last commit (already pushed):**
```bash
git revert HEAD
```

---

## Performance Issues

### Slow Build

**Solutions:**
1. Clear caches: `rm -rf node_modules/.cache`
2. Check for large unused dependencies
3. Use dynamic imports for large components

### Slow Tests

**Solutions:**
1. Run tests in parallel (default in Vitest)
2. Mock expensive operations
3. Use `--run` for single execution (skip watch mode)

---

## Quick Commands Reference

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server |
| `npm run build` | Production build |
| `npm run typecheck` | TypeScript validation |
| `npm run test` | Run tests |
| `npm run test -- --run` | Run tests once (no watch) |
| `npm run lint` | Lint check |
| `npm run gen:types` | Generate Supabase types |

---

## When All Else Fails

1. **Check ACIS memory for similar issues:**
   ```bash
   grep -i "<keyword>" .claude/memory/mistakes.json
   ```

2. **Reset to known good state:**
   ```bash
   git stash
   npm ci
   npm run build
   ```

3. **Clear all caches:**
   ```bash
   rm -rf node_modules/.cache
   rm -rf .vite
   rm -rf dist
   ```

4. **Full reinstall:**
   ```bash
   rm -rf node_modules
   npm install
   ```

5. **Document the issue:**
   - Add to `mistakes.json` if you find the solution
   - Update this file if procedure was missing
