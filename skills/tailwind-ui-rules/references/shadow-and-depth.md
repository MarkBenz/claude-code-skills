# Shadow and Depth System

Shadows communicate hierarchy, interactivity, and spatial relationships. A disciplined shadow system prevents visual noise and keeps the interface readable. This reference covers shadow levels, dark mode strategies, colored shadows, and 3D interaction effects using TailwindCSS utilities.

## Shadow Hierarchy

Each level corresponds to a specific degree of perceived elevation. Map every element in your design to exactly one level.

| Level | Class | Visual Effect | Use Cases |
|-------|-------|--------------|-----------|
| 0 | `border border-gray-200` | No shadow, defined edge | Inline elements, list items, table cells |
| 1 | `shadow-sm` | Barely perceptible lift | Cards at rest, input fields, subtle containers |
| 2 | `shadow-md` | Clear elevation | Hovered cards, active dropdowns, tooltips |
| 3 | `shadow-lg` | Prominent float | Popovers, floating panels, toasts |
| 4 | `shadow-xl` / `shadow-2xl` | Maximum elevation | Modal dialogs, command palettes, overlays |

**Rule**: Pick at most 3 of these 5 levels for your design system. Using all five creates ambiguous depth relationships where the user cannot distinguish between layers. Typical combinations are Level 0 + Level 1 + Level 3, or Level 1 + Level 2 + Level 4. The important thing is clear separation between the levels you choose.

## Card Elevation Pattern

Cards are the most common shadow-bearing element. Here is a complete card at each level with full TailwindCSS classes:

```html
<!-- Level 0: Flat card (border only) -->
<div class="rounded-lg border border-gray-200 dark:border-gray-700 p-6">
  Flat content with defined edges, no perceived lift.
</div>

<!-- Level 1: Subtle lift (default card) -->
<div class="rounded-lg shadow-sm p-6">
  Standard resting state for cards in dashboards and content layouts.
</div>

<!-- Level 2: Elevated (hovered/active) -->
<div class="rounded-lg shadow-md p-6">
  Active state or hovered card, clearly above surrounding content.
</div>

<!-- Level 3: Floating (modal/overlay) -->
<div class="rounded-lg shadow-xl p-6">
  Floats above the page. Reserved for overlays, modals, and popovers.
</div>
```

Every card in the same container or grid must share the same resting shadow level. Mixed levels within a group break visual consistency and confuse the spatial model.

## Hover Elevation

Cards that respond to hover should step up exactly one shadow level and lift slightly along the Y axis:

```html
<div class="rounded-lg shadow-sm hover:shadow-md hover:-translate-y-0.5 transition-all duration-200 p-6">
  This card lifts subtly on hover to signal interactivity.
</div>
```

Rules for hover elevation:
- Step up exactly ONE shadow level on hover (shadow-sm to shadow-md, never shadow-sm to shadow-xl).
- Combine with a subtle `hover:-translate-y-0.5` which translates to 2px. Do not use `-translate-y-1` or larger values because exaggerated movement feels unpolished and distracts from content.
- Always include `transition-all duration-200` so the shadow and transform animate together smoothly. Without the transition, the change appears as a jarring snap.

## Dark Mode Shadows

Shadows are barely visible on dark backgrounds because there is little contrast between the shadow color and the surface. You need an alternative strategy to communicate depth in dark mode.

```html
<!-- Approach 1: Remove shadows, use borders -->
<div class="shadow-sm dark:shadow-none dark:border dark:border-gray-700 rounded-lg p-6">
  Clean separation using a subtle border in dark mode.
</div>

<!-- Approach 2: Use darker, more opaque shadows -->
<div class="shadow-sm dark:shadow-lg dark:shadow-black/30 rounded-lg p-6">
  Heavier shadow that remains visible against dark surfaces.
</div>

<!-- Approach 3: Use ring for subtle definition -->
<div class="shadow-sm dark:shadow-none dark:ring-1 dark:ring-gray-700 rounded-lg p-6">
  Ring provides a crisp 1px edge without shadow rendering overhead.
</div>
```

Approach 1 or 3 are the cleanest solutions and work well in the vast majority of interfaces. Borders and rings are predictable, lightweight, and render consistently across browsers. Use Approach 2 only when you specifically need the perception of floating depth in dark mode, such as modals or command palettes that must appear above a dark backdrop.

## Colored Shadows

Colored shadows draw attention and create visual emphasis. They should be used surgically on one or two elements per page that need to stand out.

```html
<!-- DO: Primary CTA button -->
<button class="bg-blue-600 shadow-lg shadow-blue-600/25 hover:shadow-blue-600/40">
  Get Started
</button>

<!-- DO: Accent card highlighting something important -->
<div class="border-l-4 border-blue-500 shadow-md shadow-blue-500/10">
  Featured announcement or alert content.
</div>

<!-- DON'T: Neutral container -->
<div class="shadow-lg shadow-gray-500/25">  <!-- Just use shadow-lg -->
  Regular content that does not need color emphasis.
</div>

<!-- DON'T: Every card in a grid -->
<div class="shadow-md shadow-purple-500/20">  <!-- Visual chaos -->
  One of many identical cards. Colored shadow here creates noise.
</div>
```

**Rule**: Colored shadows only on 1-2 elements per page that need emphasis. Never apply them to regular containers, grid items, or repeated components. When everything glows, nothing stands out.

## 3D Hover Effects

Translate and perspective transforms can create interactive depth effects. Use them with restraint.

```html
<!-- Subtle lift (most common, safest) -->
<div class="hover:-translate-y-0.5 hover:shadow-md transition-all duration-200">
  Cards, list items, and any clickable surface.
</div>

<!-- Button press effect -->
<button class="hover:-translate-y-0.5 active:translate-y-0 transition-transform duration-150">
  Lifts on hover, returns to baseline on click for tactile feedback.
</button>

<!-- Perspective tilt (use sparingly) -->
<div class="hover:[transform:perspective(800px)_rotateY(-2deg)] transition-transform duration-300">
  Featured hero card or single showcase element only.
</div>
```

Rules for 3D effects:
- `translate-y`: maximum `-0.5` (2px) for standard cards. Use `-1` (4px) only for large hero elements or feature showcases.
- Always pair movement with `transition-transform` or `transition-all` so changes animate smoothly.
- Perspective tilt effects belong on single featured elements only. Never apply them to items in a grid or repeated list because simultaneous tilting across multiple elements looks chaotic.
- Use `active:translate-y-0` on buttons to create a satisfying "press down" feedback that complements the hover lift.

## Shadow Anti-Patterns

Common mistakes that undermine a coherent depth system:

| Don't | Why | Do Instead |
|-------|-----|-----------|
| `shadow-2xl` on small badges | Disproportionate to element size, looks like a rendering bug | `shadow-sm` or no shadow |
| Different shadow levels on cards in the same grid | Breaks visual consistency, implies false hierarchy | Same shadow level for all cards in a group |
| `shadow-inner` on cards | Creates a sunken look that confuses the depth model | Reserve `shadow-inner` for inset inputs and pressed states only |
| Mixing shadow directions | Some shadows up, some down looks broken and inconsistent | Maintain consistent shadow direction across all elements |
| `shadow-xl` + `border-2` + `ring-2` | Triple depth cues compete with each other and create visual noise | Pick one depth technique per element |

A well-structured shadow system uses fewer levels, applies them consistently, and adapts cleanly for dark mode. When in doubt, use less shadow rather than more.
