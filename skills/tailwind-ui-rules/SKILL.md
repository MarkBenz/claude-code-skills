---
name: tailwind-ui-rules
description: >-
  This skill provides concrete TailwindCSS implementation rules for building
  production-grade frontends. Use when the user asks to build a component with
  TailwindCSS, fix layout alignment, add shadows or depth effects, create a
  responsive layout, add hover or focus states, fix spacing issues, build a
  card or dashboard layout, create a bento grid, add glassmorphism effects,
  implement skeleton loading, add dark mode, use container queries, implement
  fluid typography, or when working on any frontend implementation using
  Tailwind CSS utility classes. Complements the frontend-design aesthetic
  skill with actionable implementation patterns and anti-patterns.
---

# TailwindCSS UI/UX Implementation Rules

This skill provides **concrete implementation rules** for TailwindCSS frontends. It complements the `frontend-design` skill which handles aesthetic direction (tone, typography choices, color philosophy, motion). Apply these rules **after** choosing the aesthetic direction.

The `frontend-design` skill answers: *What should the design feel like?*
This skill answers: *How do you implement it correctly in TailwindCSS?*

Always follow every rule in this document when generating TailwindCSS code. Violations of these rules are implementation bugs.

---

## 1. Shadow and Depth System

Define a **maximum of 3 shadow levels** per design system. More levels create visual noise.

| Level | Class | Use Case |
|-------|-------|----------|
| 0 — Flat | `border border-gray-200` | Default containers, inline elements |
| 1 — Subtle | `shadow-sm` | Cards at rest, input fields |
| 2 — Elevated | `shadow-md` or `shadow-lg` | Hovered cards, dropdowns, popovers |
| 3 — Floating | `shadow-xl` or `shadow-2xl` | Modals, command palettes, overlays |

**Rules:**
- Never apply `shadow-lg` or above to small inline elements (badges, tags, chips).
- Use `shadow-sm` as the default card shadow — not `shadow-md`.
- Hover elevation: step up exactly one level (e.g., `shadow-sm` → `hover:shadow-md`).
- Always pair shadow transitions with `transition-shadow duration-200`.
- Dark mode: reduce or remove shadows. Use `dark:shadow-none` or `dark:shadow-black/20` with a reduced level.
- Colored shadows (`shadow-blue-500/25`) only on primary action elements — never on neutral containers.

See `references/shadow-and-depth.md` for full code examples, dark mode patterns, 3D hover effects, glassmorphism, neumorphism, and gradient techniques.

---

## 2. Layout Alignment

All sibling elements within a section **must share consistent alignment edges**. Misalignment by even 1px breaks visual rhythm.

**Core rules:**
- Use `gap-*` for spacing between siblings. Never use individual `mb-*` or `mr-*` on each child — this creates inconsistent spacing when items wrap or reorder.
- Page container pattern: `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8`. Use this (or a similar constrained container) on every page-level layout.
- **Flex** for one-dimensional flow (nav bars, button groups, card rows). **Grid** for two-dimensional layouts (dashboards, galleries, form layouts). Never nest multiple flex containers to simulate a grid.
- Left-align body text. Never center paragraphs of body copy — only center headings and short labels when the design calls for it.
- All content blocks within a section share the same left padding edge. If a heading and its paragraph have different left offsets, it is a bug.
- Avoid `w-full` on elements inside flex containers when you actually need `flex-1` or `flex-grow`.

**Overflow and shadow clipping:** Scrollable containers (`overflow-y-auto`) clip child shadows. Fix with the negative margin + padding trick: `-mx-4 px-4` on the scroll container creates space for shadows to render. Match the margin size to your shadow spread.

See `references/layout-and-spacing.md` for flex patterns, grid patterns, bento grids, container queries, overflow/shadow clipping solutions, flush alignment examples, and spacing scale reference.

---

## 3. Spacing System

**All spacing must come from the Tailwind scale** (multiples of 4px). Never use arbitrary values.

| Don't | Do | Why |
|-------|----|-----|
| `p-[13px]` | `p-3` (12px) or `p-3.5` (14px) | Arbitrary values break the spacing rhythm |
| `mt-[22px]` | `mt-5` (20px) or `mt-6` (24px) | Stay on the 4px grid |
| `gap-[7px]` | `gap-2` (8px) | Consistent spacing scale |

**Spacing rhythm:** Pick 3-4 spacing values and reuse them everywhere:
- **Tight** (component internals): `p-3` or `p-4`, `gap-2` or `gap-3`
- **Comfortable** (between components): `p-6`, `gap-4` or `gap-6`
- **Spacious** (between sections): `py-12 sm:py-16 lg:py-24`

**Consistency rule:** If two cards in the same row have different internal padding, it is a bug. All instances of the same component type must use identical spacing.

See `references/layout-and-spacing.md` for the complete spacing scale and section rhythm patterns.

---

## 4. Responsive Design

**Always mobile-first.** Write base styles for the smallest screen, then layer larger breakpoints.

**Breakpoint usage:**
- No prefix: Mobile (< 640px) — this is your **base**
- `sm:` (640px+): Landscape phones, small tablets
- `md:` (768px+): Tablets
- `lg:` (1024px+): Desktops
- `xl:` (1280px+): Large desktops — use sparingly

**Rules:**
- Grid column progression: `grid-cols-1` → `sm:grid-cols-2` → `lg:grid-cols-3` → `xl:grid-cols-4`
- Typography must scale: at minimum increase heading sizes at `lg:` breakpoint.
- Show/hide: use `hidden lg:block` (not `block lg:hidden`) — define what's visible by default, then add at larger sizes.
- Touch targets: all interactive elements must be at minimum `h-10 w-10` (40px) on mobile for accessible tap targets.
- Never hard-code widths (`w-[800px]`). Use `max-w-*` utilities or responsive fractions.

See `references/responsive-patterns.md` for complete responsive navigation, card grid, typography scale, and fluid typography (clamp) patterns.

---

## 5. Interactive States

**Every interactive element must define:** hover, focus-visible, active, and disabled states.

**Standard button pattern:**
```html
<button class="
  px-4 py-2 rounded-md font-medium
  bg-blue-600 text-white
  hover:bg-blue-700
  active:bg-blue-800
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:cursor-not-allowed
  transition-colors duration-150
">
  Button
</button>
```

**Rules:**
- Use `focus-visible:` not `focus:`. Focus rings should only appear for keyboard navigation, not mouse clicks.
- Always include `transition-colors` or `transition-all` with `duration-150` (micro-interactions) or `duration-300` (major transitions).
- Disabled state: `disabled:opacity-50 disabled:cursor-not-allowed`. Never remove the element — keep it visible but inactive.
- Hover effects on cards: step up shadow one level + subtle `translate-y` or border color change. Never use only color change on a card hover.
- Form inputs need: default border, `focus-visible:ring-2`, error state (`border-red-500 focus-visible:ring-red-500`), and disabled state.

See `references/interactive-states.md` for complete button, input, card, toggle state patterns, skeleton loading, prefers-reduced-motion, and advanced card variants.

---

## 6. Common AI-Generated Mistakes — Quick Reference

| Mistake | Why It's Wrong | Correct Pattern |
|---------|---------------|-----------------|
| `shadow-xl` on every card | Visual noise, no hierarchy | `shadow-sm` default, `shadow-md` on hover |
| Mixed `mb-4` and `mb-6` between siblings | Inconsistent rhythm | Use `gap-4` on parent |
| `p-[17px]` | Breaks 4px grid | `p-4` (16px) or `p-5` (20px) |
| Missing `focus-visible:` on buttons | Inaccessible to keyboard users | Add `focus-visible:ring-2 focus-visible:ring-offset-2` |
| `<div onClick>` instead of `<button>` | No keyboard/screen-reader support | Use `<button>` or `<a>` |
| `z-[9999]` | Z-index wars, unmaintainable | Define 3-4 z-index levels: `z-10`, `z-20`, `z-30`, `z-50` |
| Centered body paragraphs | Hard to read, looks unprofessional | `text-left` for body copy |
| No `dark:` variants | Broken dark mode | Add `dark:` for bg, text, border, shadow |
| State changes without transitions | Jarring, feels broken | `transition-colors duration-150` |
| Desktop-first classes (`block sm:hidden`) | Backwards; mobile undefined | `hidden sm:block` (mobile-first) |
| Single dark bg for everything | No depth in dark mode | 3-level surface: `bg-gray-950` / `bg-gray-900` / `bg-gray-800` |
| Animations without `motion-safe:` | Ignores vestibular disorders | `motion-safe:animate-bounce`, `motion-safe:hover:-translate-y-0.5` |

See `references/anti-patterns.md` for detailed before/after code examples for each mistake (12 anti-patterns total).

---

## Reference Files

Consult these for detailed code patterns when implementing specific features:

- `references/shadow-and-depth.md` — Shadow hierarchy, dark mode shadows, colored shadows, 3D hover effects, **glassmorphism**, **neumorphism**, **modern gradients**
- `references/layout-and-spacing.md` — Container patterns, flex/grid layouts, flush alignment, spacing scale, **bento grids**, **container queries**
- `references/responsive-patterns.md` — Mobile-first patterns, responsive grids, typography scaling, **fluid typography with clamp()**
- `references/interactive-states.md` — Button/input/card/toggle state patterns, focus rings, transitions, **skeleton loading**, **prefers-reduced-motion**, **advanced card variants**
- `references/anti-patterns.md` — 12 common AI mistakes with wrong code → correct code examples, including **dark mode surface layers** and **motion accessibility**
