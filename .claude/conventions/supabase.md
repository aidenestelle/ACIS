# Supabase Conventions

> ACIS Supabase coding standards for database, auth, and edge functions

---

## Client Setup

### Centralize Client Instance

```typescript
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js';
import type { Database } from '@/types/supabase';

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

export const supabase = createClient<Database>(supabaseUrl, supabaseKey);
```

### Use Generated Types

```typescript
import type { Database } from '@/types/supabase';

// Table row types
type User = Database['public']['Tables']['users']['Row'];
type UserInsert = Database['public']['Tables']['users']['Insert'];
type UserUpdate = Database['public']['Tables']['users']['Update'];
```

---

## Query Patterns

### Always Handle Errors

**CRITICAL: Never assume data exists.**

```typescript
// BAD - ignores error
const { data } = await supabase.from('users').select('*');
return data; // Could be null!

// GOOD - explicit error handling
const { data, error } = await supabase
  .from('users')
  .select('*')
  .eq('id', userId);

if (error) {
  console.error('Supabase error:', error.message);
  throw new Error(`Failed to fetch user: ${error.message}`);
}

return data;
```

### Select Only Needed Columns

```typescript
// BAD - fetches everything
const { data } = await supabase.from('users').select('*');

// GOOD - fetches only needed columns
const { data } = await supabase
  .from('users')
  .select('id, name, email');
```

### Use Type-Safe Queries

```typescript
// With generated types, queries are type-safe
const { data, error } = await supabase
  .from('users')
  .select('id, name, email')
  .eq('status', 'active')
  .single();

// data is typed as User | null
if (data) {
  console.log(data.name); // TypeScript knows this exists
}
```

---

## Insert/Update/Delete

### Insert with Type Safety

```typescript
const newUser: Database['public']['Tables']['users']['Insert'] = {
  email: 'user@example.com',
  name: 'John Doe',
};

const { data, error } = await supabase
  .from('users')
  .insert(newUser)
  .select()
  .single();

if (error) throw new Error(`Insert failed: ${error.message}`);
```

### Update with Filters

```typescript
const { error } = await supabase
  .from('users')
  .update({ status: 'inactive' })
  .eq('id', userId);

if (error) throw new Error(`Update failed: ${error.message}`);
```

### Delete with Confirmation

```typescript
const { error } = await supabase
  .from('users')
  .delete()
  .eq('id', userId);

if (error) throw new Error(`Delete failed: ${error.message}`);
```

---

## Authentication

### Auth State Management

```typescript
import { useEffect, useState } from 'react';
import { supabase } from '@/lib/supabase';
import type { User } from '@supabase/supabase-js';

export function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Get initial session
    supabase.auth.getSession().then(({ data: { session } }) => {
      setUser(session?.user ?? null);
      setLoading(false);
    });

    // Listen for auth changes
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (_event, session) => {
        setUser(session?.user ?? null);
      }
    );

    return () => subscription.unsubscribe();
  }, []);

  return { user, loading };
}
```

### Sign In/Out

```typescript
async function signIn(email: string, password: string) {
  const { error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });

  if (error) throw new Error(`Sign in failed: ${error.message}`);
}

async function signOut() {
  const { error } = await supabase.auth.signOut();
  if (error) throw new Error(`Sign out failed: ${error.message}`);
}
```

---

## Real-Time Subscriptions

### Subscribe to Changes

```typescript
useEffect(() => {
  const channel = supabase
    .channel('table-changes')
    .on(
      'postgres_changes',
      { event: '*', schema: 'public', table: 'messages' },
      (payload) => {
        console.log('Change received:', payload);
        // Update local state
      }
    )
    .subscribe();

  // CRITICAL: Always cleanup
  return () => {
    supabase.removeChannel(channel);
  };
}, []);
```

---

## Storage

### Upload Files

```typescript
async function uploadFile(file: File, path: string) {
  const { data, error } = await supabase.storage
    .from('bucket-name')
    .upload(path, file);

  if (error) throw new Error(`Upload failed: ${error.message}`);
  return data;
}
```

### Get Public URL

```typescript
const { data } = supabase.storage
  .from('bucket-name')
  .getPublicUrl('path/to/file.jpg');

return data.publicUrl;
```

---

## Edge Functions

### Invoke Functions

```typescript
const { data, error } = await supabase.functions.invoke('function-name', {
  body: { param1: 'value' },
});

if (error) throw new Error(`Function error: ${error.message}`);
return data;
```

---

## Environment Variables

### Required Variables

```bash
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

**NEVER commit these to version control.**

---

## Common Mistakes to Avoid

| Mistake | Prevention |
|---------|------------|
| Ignoring error object | Always check `if (error)` after every query |
| Using `any` for data | Use generated types from `supabase gen types` |
| Missing subscription cleanup | Always return cleanup in useEffect |
| Hardcoding credentials | Use environment variables |
| Selecting all columns | Specify only needed columns |

---

## Related Files

- `mistakes.json` → ERR-005 (secrets), ERR-006 (error handling)
- `CLAUDE.md` → Emergency Procedures (Supabase errors)
