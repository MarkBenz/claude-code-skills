# Claude Code Skills

A collection of custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills that extend Claude's capabilities for frontend development.

## Available Skills

### tailwind-ui-rules

Concrete TailwindCSS UI/UX implementation rules covering shadows, layout alignment, spacing systems, responsive patterns, and interactive states. Complements the official Anthropic `frontend-design` skill with actionable code-level guidance.

**What the Anthropic `frontend-design` skill covers:** Aesthetic direction — tone, typography choices, color philosophy, motion, spatial composition.

**What this skill covers:** Correct TailwindCSS implementation — shadow hierarchy, flush alignment, spacing consistency, responsive patterns, interactive states, and common AI-generated mistakes to avoid.

#### Reference Files

| File | Content |
|------|---------|
| `shadow-and-depth.md` | Shadow hierarchy, dark mode shadows, colored shadows, 3D hover effects |
| `layout-and-spacing.md` | Container patterns, flex/grid layouts, flush alignment, spacing scale |
| `responsive-patterns.md` | Mobile-first patterns, responsive grids, typography scaling |
| `interactive-states.md` | Button/input/card/toggle state patterns, focus rings, transitions |
| `anti-patterns.md` | 10 common AI mistakes with wrong code → correct code examples |

## Installation

### As a local plugin

```bash
claude --plugin-dir /path/to/this/repo
```

### As a global plugin

Add to your Claude Code settings:

```json
{
  "plugins": ["/path/to/this/repo"]
}
```

## Contributing

Feel free to open issues or PRs to suggest new rules, patterns, or additional skills.

## License

MIT
