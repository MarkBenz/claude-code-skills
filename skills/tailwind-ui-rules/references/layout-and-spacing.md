# Layout and Spacing System

This reference covers the foundational layout patterns, spacing conventions, and alignment rules used in TailwindCSS-based interfaces. Consistent layout and spacing are the single largest contributor to a polished, professional UI. Inconsistent spacing is the most common source of visual "jank" in otherwise well-designed pages.

## Page Container Pattern

The standard page container that all page-level layouts should use:

```html
<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
  <!-- Page content -->
</div>
```

Each part serves a specific purpose:

- **mx-auto**: Centers the container horizontally within its parent. Without this, the container would be left-aligned once its max-width kicks in on wide screens.
- **max-w-7xl** (1280px): Prevents content from stretching to absurd line lengths on ultrawide monitors. Long lines of text are hard to read, and layouts that span 2560px look broken. This cap keeps content readable and visually grounded.
- **px-4 sm:px-6 lg:px-8**: Progressive horizontal padding that gives content breathing room from the viewport edges. On small screens, 16px is enough. On tablets, 24px provides more space. On desktop, 32px creates comfortable margins. This prevents content from touching the edges of the browser window at any screen size.

### Container Variants for Different Page Types

Not every page should use the same max-width:

- **Narrow content** (articles, blog posts, settings forms): Use `max-w-2xl` (672px) or `max-w-3xl` (768px). Long-form text is most readable at 50-75 characters per line, and these widths achieve that.
- **Dashboard layouts**: Use `max-w-full` with a fixed-width sidebar. The content area fills the remaining space. The sidebar provides the visual constraint instead of a max-width.
- **Marketing and landing pages**: Use `max-w-7xl` (1280px). Marketing pages often use full-width background sections with constrained inner content, so the container is applied per-section rather than wrapping the entire page.

## Spacing Scale Reference

Tailwind uses a spacing scale based on multiples of 0.25rem (4px). This table maps every commonly used value to its pixel equivalent and typical usage:

| Tailwind | rem | px | Typical Use |
|----------|-----|-----|-------------|
| 0.5 | 0.125rem | 2px | Icon-to-text gap |
| 1 | 0.25rem | 4px | Tight inline spacing |
| 1.5 | 0.375rem | 6px | Compact component gaps |
| 2 | 0.5rem | 8px | Small gaps, tag padding, badge padding |
| 3 | 0.75rem | 12px | Button padding-y, compact card padding |
| 4 | 1rem | 16px | Standard component padding, common gap |
| 5 | 1.25rem | 20px | Comfortable padding |
| 6 | 1.5rem | 24px | Card padding, section gap |
| 8 | 2rem | 32px | Large component padding |
| 10 | 2.5rem | 40px | Between major blocks |
| 12 | 3rem | 48px | Section vertical padding (mobile) |
| 16 | 4rem | 64px | Section vertical padding (desktop) |
| 20 | 5rem | 80px | Hero spacing |
| 24 | 6rem | 96px | Large hero spacing |

The key principle is to always use values from this scale rather than arbitrary values. The scale creates visual consistency because repeated spacing values produce a sense of rhythm and order.

## Flush Alignment Rules

All content blocks within a visual section must share the same left and right edges. When a heading and the content below it have different horizontal positions, the layout looks broken even if each individual element is well-styled. This is called "flush alignment" and it is non-negotiable.

**WRONG: Heading and cards have different left edges**

```html
<!-- WRONG: Heading and cards have different left edges -->
<div class="px-8">
  <h2 class="text-2xl font-bold">Our Products</h2>
</div>
<div class="px-4">
  <div class="grid grid-cols-3 gap-4">
    <div class="p-4">Card 1</div>
    <div class="p-4">Card 2</div>
    <div class="p-4">Card 3</div>
  </div>
</div>
```

The heading has 32px of padding on each side while the card grid has only 16px. The heading text and the card edges do not line up, creating a ragged, unintentional look.

**CORRECT: Shared container, consistent edges**

```html
<!-- CORRECT: Shared container, consistent edges -->
<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
  <h2 class="text-2xl font-bold mb-6">Our Products</h2>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <div class="p-6">Card 1</div>
    <div class="p-6">Card 2</div>
    <div class="p-6">Card 3</div>
  </div>
</div>
```

**Rule**: One container controls the page edges. Everything inside inherits the same alignment. Never apply horizontal padding to multiple nested wrappers.

## Gap vs Margin

For spacing between sibling elements, `gap-*` on the parent is superior to individual margins on children. Here is why:

**WRONG: Margins on children**

```html
<div class="flex flex-col">
  <div class="mb-4">Item 1</div>
  <div class="mb-6">Item 2</div>  <!-- inconsistent! -->
  <div class="mb-4">Item 3</div>
  <div>Item 4</div>  <!-- last item has no margin -- correct, but spacing is inconsistent -->
</div>
```

**CORRECT: Gap on parent**

```html
<div class="flex flex-col gap-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
  <div>Item 4</div>
</div>
```

The benefits of gap over margin:

- **No trailing space**: Gap never applies after the last item, so there is no unwanted extra margin at the bottom of a list.
- **Single source of truth**: Changing the spacing between items means editing one class on the parent rather than updating every child.
- **Works with wrapping**: When used with `flex-wrap`, gap handles both row gaps and column gaps correctly. Margins require hacks to avoid extra space on wrapping edges.

**When margin IS the right choice**:

- `mb-*` on a heading before its related content block. This creates a semantic grouping where the heading "owns" the space below it.
- `mt-auto` to push an element to the bottom of a flex container (such as a button at the bottom of a card).
- Negative margins (`-mt-4`, `-mx-2`) for intentional overlap effects like pulling an image outside its container.

## Flex Layout Patterns

### Navigation Bar

```html
<nav class="flex items-center justify-between h-16 px-4">
  <div class="flex items-center gap-2">Logo + Brand</div>
  <div class="flex items-center gap-6">Nav Links</div>
  <div class="flex items-center gap-3">Actions</div>
</nav>
```

The `justify-between` pushes the three groups to the edges and center. Each group uses `items-center` to vertically align its children, and `gap-*` to space them internally.

### Card Row (Equal Width)

```html
<div class="flex gap-6">
  <div class="flex-1 min-w-0">Card 1</div>
  <div class="flex-1 min-w-0">Card 2</div>
  <div class="flex-1 min-w-0">Card 3</div>
</div>
```

Note: `min-w-0` is critical. Without it, flex items have an implicit `min-width: auto` that prevents them from shrinking below their content size. Long text or URLs inside a card will cause it to overflow the parent. Always add `min-w-0` to flex children that contain text.

### Split Layout (Sidebar + Content)

```html
<div class="flex gap-8">
  <aside class="w-64 shrink-0">Sidebar</aside>
  <main class="flex-1 min-w-0">Main content</main>
</div>
```

The sidebar has a fixed width (`w-64` = 256px) and `shrink-0` prevents it from being compressed. The main content area takes the remaining space with `flex-1`.

## Grid Layout Patterns

### Dashboard Grid

```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
  <div class="lg:col-span-2">Wide stat</div>
  <div>Stat 2</div>
  <div>Stat 3</div>
</div>
```

Grid is ideal for dashboard layouts because `col-span-*` lets certain items take more space without breaking the rhythm of the grid.

### Form Layout

```html
<form class="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-4">
  <div class="sm:col-span-2">Full-width field</div>
  <div>First name</div>
  <div>Last name</div>
  <div>Email</div>
  <div>Phone</div>
  <div class="sm:col-span-2">Address</div>
</form>
```

Using `gap-x-6 gap-y-4` provides wider horizontal spacing between columns and tighter vertical spacing between rows, matching how forms are typically read top-to-bottom.

### Gallery Grid

```html
<div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4">
  <!-- Uniform image cards -->
</div>
```

A tighter `gap-4` works well for image galleries where the visual content should dominate the layout.

## Section Spacing Rhythm

Define a consistent vertical rhythm between major page sections. All sections on a page should use the same vertical padding pattern unless there is a deliberate reason to deviate:

```html
<section class="py-12 sm:py-16 lg:py-24">
  <!-- Hero section -->
</section>

<section class="py-12 sm:py-16 lg:py-24">
  <!-- Features section -->
</section>

<section class="py-12 sm:py-16 lg:py-20">
  <!-- CTA section (slightly less for urgency) -->
</section>
```

**Rule**: All major sections use the same `py-*` values. Variation should be intentional and rare. When every section has different vertical padding, the page feels chaotic. When they share the same values, the page has a steady, confident rhythm.

## Common Spacing Mistakes

| Mistake | Fix |
|---------|-----|
| Different padding on cards in the same grid | Use the same `p-*` class on all cards in a group |
| margin-bottom on flex children instead of gap | Use `gap-*` on the flex parent container |
| px-4 on container + px-4 on child = double padding | Container handles page edges; children use gap or internal padding only |
| Arbitrary values like `p-[13px]` | Use scale values: `p-3` (12px) or `p-3.5` (14px) |
| No padding on mobile, too much on desktop | Use progressive padding: `px-4 sm:px-6 lg:px-8` |
| Mixing spacing approaches in the same component | Pick one approach (gap or margin) per layout context and be consistent |
