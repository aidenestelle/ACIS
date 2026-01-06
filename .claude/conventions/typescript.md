# TypeScript Conventions

> ACIS TypeScript coding standards for React + Supabase projects

---

## Type Definitions

### Prefer `type` Over `interface`

Use `type` for object shapes and unions. Only use `interface` when extending or declaration merging is needed.

```typescript
// Prefer type for object shapes
type UserData = {
  id: string;
  email: string;
  createdAt: Date;
};

// Use type for unions
type Status = 'pending' | 'active' | 'completed';

// Use type for function signatures
type Handler = (event: Event) => void;

// Only use interface when extending
interface ExtendedUser extends BaseUser {
  additionalField: string;
}
```

---

## Strict Typing

### No Implicit `any`

Always type explicitly. Never let TypeScript infer `any`.

```typescript
// BAD
function process(data) { ... }

// GOOD
function process(data: InputData): OutputData { ... }
```

### Use `unknown` Instead of `any`

When type is truly unknown, use `unknown` with type guards.

```typescript
// BAD
function handle(input: any) {
  return input.value; // No safety
}

// GOOD
function handle(input: unknown): string {
  if (typeof input === 'object' && input !== null && 'value' in input) {
    return String((input as { value: unknown }).value);
  }
  throw new Error('Invalid input');
}
```

---

## Exported Functions

### Explicit Return Types

All exported functions must have explicit return types.

```typescript
// BAD - return type inferred
export function processData(input: Input) {
  return { result: input.value };
}

// GOOD - return type explicit
export function processData(input: Input): ProcessResult {
  return { result: input.value };
}
```

---

## Event Handlers

### Type Event Parameters

Always type event handler parameters.

```typescript
// BAD
const handleClick = (e) => { ... }

// GOOD
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => { ... }
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => { ... }
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => { ... }
```

---

## Utility Types

### Use Built-in Utility Types

```typescript
// Partial - all properties optional
type PartialUser = Partial<User>;

// Required - all properties required
type RequiredConfig = Required<Config>;

// Pick - subset of properties
type UserPreview = Pick<User, 'id' | 'name'>;

// Omit - exclude properties
type UserWithoutPassword = Omit<User, 'password'>;

// Record - dictionary type
type UserMap = Record<string, User>;
```

---

## Generics

### Use Descriptive Generic Names

```typescript
// BAD
function wrap<T>(value: T): { data: T } { ... }

// GOOD - for simple cases, T is acceptable
function wrap<T>(value: T): { data: T } { ... }

// BETTER - for complex cases, use descriptive names
function fetchAndTransform<TInput, TOutput>(
  fetcher: () => Promise<TInput>,
  transformer: (input: TInput) => TOutput
): Promise<TOutput> { ... }
```

---

## Null Handling

### Prefer Nullish Coalescing

```typescript
// BAD
const value = input || 'default';  // Treats 0 and '' as falsy

// GOOD
const value = input ?? 'default';  // Only null/undefined trigger default
```

### Use Optional Chaining

```typescript
// BAD
const name = user && user.profile && user.profile.name;

// GOOD
const name = user?.profile?.name;
```

---

## Common Mistakes to Avoid

| Mistake | Prevention |
|---------|------------|
| Using `any` | Use `unknown` with type guards |
| Implicit any in callbacks | Always type callback parameters |
| Missing return types | Add explicit return types to exports |
| Object mutation | Use spread operator for updates |
| Ignoring null/undefined | Use optional chaining and nullish coalescing |

---

## Related Files

- `mistakes.json` → ERR-001 (any type), ERR-007 (implicit any in events)
- `CLAUDE.md` → Anti-Patterns section
