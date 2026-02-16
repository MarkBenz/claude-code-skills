# Interactive States

Every interactive element must provide visual feedback for hover, focus-visible, active, and disabled states. Missing states make interfaces feel broken and inaccessible. Users rely on these cues to understand what is clickable, what is focused, and what is unavailable. A button without a hover state feels dead. An input without a focus ring is invisible to keyboard users. Treat state coverage as a requirement, not an enhancement.

## Button States

### Primary Button
```html
<button class="
  px-4 py-2.5 rounded-lg font-medium text-sm
  bg-blue-600 text-white
  hover:bg-blue-700
  active:bg-blue-800 active:scale-[0.98]
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-blue-600
  transition-all duration-150
">
  Primary Action
</button>
```

### Secondary Button (outline)
```html
<button class="
  px-4 py-2.5 rounded-lg font-medium text-sm
  border border-gray-300 text-gray-700 bg-white
  hover:bg-gray-50 hover:border-gray-400
  active:bg-gray-100
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:cursor-not-allowed
  dark:border-gray-600 dark:text-gray-300 dark:bg-transparent
  dark:hover:bg-gray-800 dark:hover:border-gray-500
  transition-all duration-150
">
  Secondary Action
</button>
```

### Ghost/Subtle Button
```html
<button class="
  px-3 py-2 rounded-md font-medium text-sm
  text-gray-600
  hover:bg-gray-100 hover:text-gray-900
  active:bg-gray-200
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2
  dark:text-gray-400 dark:hover:bg-gray-800 dark:hover:text-gray-200
  transition-all duration-150
">
  Subtle Action
</button>
```

### Destructive Button
```html
<button class="
  px-4 py-2.5 rounded-lg font-medium text-sm
  bg-red-600 text-white
  hover:bg-red-700
  active:bg-red-800
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-red-500 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:cursor-not-allowed
  transition-colors duration-150
">
  Delete
</button>
```

**Button State Rules:**
- `disabled:hover:` must reset to the default bg color (prevent hover effect on disabled buttons).
- `active:scale-[0.98]` is an optional press-down effect. Use on primary buttons only, not on small icon buttons.
- Transition duration: 150ms for buttons (fast, snappy feedback).

## Form Input States

### Text Input
```html
<input class="
  w-full px-3 py-2 rounded-md text-sm
  border border-gray-300 bg-white text-gray-900
  placeholder:text-gray-400
  hover:border-gray-400
  focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500
  disabled:bg-gray-50 disabled:text-gray-500 disabled:cursor-not-allowed
  dark:border-gray-600 dark:bg-gray-900 dark:text-gray-100
  dark:focus:ring-blue-400
  transition-colors duration-150
" placeholder="Enter value..." />
```

Note: Use `focus:` (not `focus-visible:`) for form inputs because inputs always have visible focus when clicked AND when tabbed.

### Error State Input
```html
<div>
  <input class="
    w-full px-3 py-2 rounded-md text-sm
    border border-red-500 bg-white text-gray-900
    focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-red-500
    dark:border-red-400 dark:bg-gray-900
    transition-colors duration-150
  " />
  <p class="mt-1.5 text-sm text-red-600 dark:text-red-400">This field is required.</p>
</div>
```

### Select Input
```html
<select class="
  w-full px-3 py-2 rounded-md text-sm appearance-none
  border border-gray-300 bg-white text-gray-900
  hover:border-gray-400
  focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500
  disabled:bg-gray-50 disabled:cursor-not-allowed
  dark:border-gray-600 dark:bg-gray-900 dark:text-gray-100
  transition-colors duration-150
">
```

## Card Hover Effects

### Lift Effect (recommended default)
```html
<div class="
  rounded-lg border border-gray-200 p-6
  shadow-sm
  hover:shadow-md hover:-translate-y-0.5
  transition-all duration-200
  cursor-pointer
">
```

### Border Highlight Effect
```html
<div class="
  rounded-lg border border-gray-200 p-6
  hover:border-blue-500
  transition-colors duration-200
  cursor-pointer
">
```

### Background Shift Effect
```html
<div class="
  rounded-lg border border-gray-200 p-6
  hover:bg-gray-50 dark:hover:bg-gray-800
  transition-colors duration-150
  cursor-pointer
">
```

**Card Rules:**
- Pick ONE hover effect per card type. Never combine lift + border highlight + background shift on the same card.
- Add `cursor-pointer` to any clickable card.
- Use `duration-200` for cards (slightly slower than buttons, feels more natural for larger elements).

## Focus Ring Patterns

The ring utilities in Tailwind provide consistent, accessible focus indicators across all interactive elements.

```html
<!-- Standard focus ring -->
focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2

<!-- Focus ring with dark mode offset color -->
focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-900

<!-- Inset focus ring (for elements where offset doesn't look right) -->
focus-visible:ring-2 focus-visible:ring-inset focus-visible:ring-blue-500
```

Rules:
- `ring-offset-2` creates a gap between element and ring. This looks cleaner than a ring flush against the element edge.
- Dark mode: the ring-offset color defaults to white. Override with `dark:ring-offset-gray-900` to match the dark background.
- `ring-inset` should be used on elements that sit flush against edges, such as table cells and nav items within a container.

## Transition Timing Guide

| Element Type | Duration | Class |
|-------------|----------|-------|
| Buttons, links | 150ms | `transition-colors duration-150` |
| Cards, panels | 200ms | `transition-all duration-200` |
| Modals, overlays | 300ms | `transition-all duration-300` |
| Page transitions | 500ms | `transition-opacity duration-500` |

Rule: Smaller elements get shorter durations. Larger movements get longer durations. Never exceed 500ms for any UI transition. Anything longer feels sluggish and degrades perceived performance.
