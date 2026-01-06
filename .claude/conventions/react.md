# React Conventions

> ACIS React coding standards for functional components with hooks

---

## Component Structure

### Functional Components Only

Use functional components with hooks. No class components for new code.

```typescript
type ComponentProps = {
  title: string;
  onAction: () => void;
  children?: React.ReactNode;
};

export function ComponentName({ title, onAction, children }: ComponentProps) {
  const [state, setState] = useState<StateType>(initialValue);

  useEffect(() => {
    // Effect logic
    return () => {
      // Cleanup - ALWAYS include for subscriptions/timers
    };
  }, [dependencies]);

  return (
    <div className="flex items-center gap-4 p-4">
      <h1>{title}</h1>
      {children}
    </div>
  );
}
```

---

## Hooks Rules

### useEffect

**Always include dependency array:**
```typescript
// BAD - runs every render
useEffect(() => { ... });

// GOOD - runs once on mount
useEffect(() => { ... }, []);

// GOOD - runs when dependencies change
useEffect(() => { ... }, [userId, status]);
```

**Always include cleanup for side effects:**
```typescript
useEffect(() => {
  const subscription = api.subscribe(handler);

  // REQUIRED: cleanup function
  return () => {
    subscription.unsubscribe();
  };
}, [handler]);
```

**Async in useEffect:**
```typescript
useEffect(() => {
  let isMounted = true;

  async function fetchData() {
    const result = await api.getData();
    if (isMounted) {
      setData(result);
    }
  }

  fetchData();

  return () => {
    isMounted = false;
  };
}, []);
```

### useState

**Type state explicitly:**
```typescript
// Simple types
const [count, setCount] = useState<number>(0);

// Complex types
const [user, setUser] = useState<User | null>(null);

// Arrays
const [items, setItems] = useState<Item[]>([]);
```

**Never mutate state directly:**
```typescript
// BAD
state.items.push(newItem);
setState(state);

// GOOD
setState(prev => ({
  ...prev,
  items: [...prev.items, newItem]
}));
```

---

## Props

### Props Typing

```typescript
// Use type for props (not interface)
type ButtonProps = {
  label: string;
  onClick: () => void;
  disabled?: boolean;
  variant?: 'primary' | 'secondary';
};
```

### Destructure Props

```typescript
// BAD
function Button(props: ButtonProps) {
  return <button onClick={props.onClick}>{props.label}</button>;
}

// GOOD
function Button({ label, onClick, disabled = false }: ButtonProps) {
  return <button onClick={onClick} disabled={disabled}>{label}</button>;
}
```

---

## Keys in Lists

**Never use array index as key:**
```typescript
// BAD
{items.map((item, index) => (
  <Item key={index} {...item} />
))}

// GOOD
{items.map(item => (
  <Item key={item.id} {...item} />
))}
```

---

## Event Handlers

### Naming Convention

Prefix with `handle`:
```typescript
const handleClick = () => { ... };
const handleSubmit = (e: React.FormEvent) => { ... };
const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => { ... };
```

### Type Events Properly

```typescript
// Mouse events
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => { ... };

// Form events
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  // ...
};

// Input events
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};
```

---

## Conditional Rendering

```typescript
// Short-circuit for simple conditions
{isLoading && <Spinner />}

// Ternary for either/or
{isLoggedIn ? <Dashboard /> : <Login />}

// Early return for complex conditions
if (!user) return <Loading />;
if (error) return <Error message={error} />;
return <Content user={user} />;
```

---

## Performance

### Memoization

Use when actually needed, not prematurely:
```typescript
// Expensive calculations
const sortedItems = useMemo(() =>
  items.sort((a, b) => a.name.localeCompare(b.name)),
  [items]
);

// Callbacks passed to child components
const handleSave = useCallback(() => {
  saveData(formData);
}, [formData]);

// Components with expensive renders
const MemoizedComponent = React.memo(ExpensiveComponent);
```

---

## Common Mistakes to Avoid

| Mistake | Prevention |
|---------|------------|
| Missing useEffect cleanup | Always return cleanup for subscriptions/timers |
| Missing dependency array | Every useEffect needs dependencies |
| Array index as key | Use stable unique IDs |
| Direct state mutation | Use spread operator or immer |
| Prop drilling | Use Context after 3 levels |

---

## Related Files

- `mistakes.json` → ERR-002, ERR-003, ERR-004, ERR-008, ERR-009, ERR-010
- `conventions/typescript.md` → Type patterns
