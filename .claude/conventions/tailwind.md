# Tailwind CSS Conventions

> ACIS Tailwind styling standards for consistent, maintainable UI

---

## Class Organization

### Order Classes by Category

Apply classes in this order for consistency and readability:

1. **Layout** (display, position, flex, grid)
2. **Spacing** (margin, padding, gap)
3. **Sizing** (width, height)
4. **Colors** (background, text, border colors)
5. **Typography** (font, text alignment)
6. **Effects** (shadow, opacity, transitions)
7. **States** (hover, focus, active)

```tsx
// GOOD - organized order
<div className="flex items-center justify-between p-4 w-full bg-white text-gray-900 rounded-lg shadow-md hover:shadow-lg transition-shadow">
  {/* Content */}
</div>
```

---

## Responsive Design

### Mobile-First Approach

Start with mobile styles, add breakpoint modifiers for larger screens.

```tsx
// Mobile-first responsive
<div className="w-full md:w-1/2 lg:w-1/3 xl:w-1/4">
  {/* Content */}
</div>

// Responsive text
<h1 className="text-xl md:text-2xl lg:text-4xl">
  Heading
</h1>

// Responsive padding
<section className="p-4 md:p-6 lg:p-8">
  {/* Content */}
</section>
```

### Breakpoint Reference

| Prefix | Min Width | Device |
|--------|-----------|--------|
| (none) | 0px | Mobile |
| `sm:` | 640px | Small tablet |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Laptop |
| `xl:` | 1280px | Desktop |
| `2xl:` | 1536px | Large desktop |

---

## Flexbox Patterns

### Common Layouts

```tsx
// Center content
<div className="flex items-center justify-center">

// Space between
<div className="flex items-center justify-between">

// Vertical stack
<div className="flex flex-col gap-4">

// Horizontal list
<div className="flex flex-row gap-2">

// Wrap items
<div className="flex flex-wrap gap-4">
```

---

## Grid Patterns

```tsx
// Basic grid
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">

// Auto-fit responsive
<div className="grid grid-cols-[repeat(auto-fit,minmax(250px,1fr))] gap-4">

// Fixed columns with gap
<div className="grid grid-cols-12 gap-4">
  <div className="col-span-8">Main</div>
  <div className="col-span-4">Sidebar</div>
</div>
```

---

## Spacing

### Consistent Spacing Scale

Use Tailwind's spacing scale consistently:

| Class | Size | Use Case |
|-------|------|----------|
| `gap-1`, `p-1` | 0.25rem | Tight spacing |
| `gap-2`, `p-2` | 0.5rem | Compact elements |
| `gap-4`, `p-4` | 1rem | Standard spacing |
| `gap-6`, `p-6` | 1.5rem | Comfortable spacing |
| `gap-8`, `p-8` | 2rem | Section spacing |

---

## Accessibility

### Focus States

Always include visible focus states:

```tsx
// Button with focus ring
<button className="focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
  Click me
</button>

// Input with focus
<input className="border border-gray-300 focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none" />
```

### Color Contrast

Ensure sufficient contrast between text and background:

```tsx
// Good contrast
<p className="text-gray-900 bg-white">
<p className="text-white bg-gray-900">
<p className="text-gray-700 bg-gray-100">

// Bad contrast - avoid
<p className="text-gray-400 bg-gray-200">
```

---

## Interactive States

### Hover and Active

```tsx
// Button states
<button className="bg-blue-500 hover:bg-blue-600 active:bg-blue-700 transition-colors">

// Card hover
<div className="bg-white hover:shadow-lg transition-shadow cursor-pointer">

// Link states
<a className="text-blue-500 hover:text-blue-700 hover:underline">
```

### Transitions

```tsx
// Smooth transitions
<div className="transition-all duration-200 ease-in-out">
<div className="transition-colors duration-150">
<div className="transition-shadow duration-200">
<div className="transition-transform hover:scale-105">
```

---

## Component Patterns

### Cards

```tsx
<div className="bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow">
  <h3 className="text-lg font-semibold text-gray-900">Title</h3>
  <p className="text-gray-600 mt-2">Description</p>
</div>
```

### Buttons

```tsx
// Primary button
<button className="px-4 py-2 bg-blue-500 text-white font-medium rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors">
  Primary
</button>

// Secondary button
<button className="px-4 py-2 bg-gray-200 text-gray-900 font-medium rounded-lg hover:bg-gray-300 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 transition-colors">
  Secondary
</button>
```

### Form Inputs

```tsx
<input
  type="text"
  className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none transition-colors"
  placeholder="Enter text..."
/>
```

---

## Dark Mode

### Using Dark Mode Classes

```tsx
// Background
<div className="bg-white dark:bg-gray-900">

// Text
<p className="text-gray-900 dark:text-gray-100">

// Border
<div className="border-gray-200 dark:border-gray-700">
```

---

## Common Mistakes to Avoid

| Mistake | Prevention |
|---------|------------|
| Inline styles | Use Tailwind classes only |
| Arbitrary values | Stick to design scale |
| Missing responsive | Always consider mobile |
| No focus states | Add focus:ring for interactive elements |
| Inconsistent spacing | Use spacing scale consistently |

---

## Related Files

- `CLAUDE.md` → Stack Awareness section
- `conventions/react.md` → Component structure
