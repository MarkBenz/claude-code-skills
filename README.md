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
| `shadow-and-depth.md` | Shadow hierarchy, dark mode shadows, colored shadows, 3D hover effects, glassmorphism, neumorphism, modern gradients |
| `layout-and-spacing.md` | Container patterns, flex/grid layouts, flush alignment, spacing scale, bento grids, container queries, overflow/shadow clipping |
| `responsive-patterns.md` | Mobile-first patterns, responsive grids, typography scaling, fluid typography with clamp() |
| `interactive-states.md` | Button/input/card/toggle state patterns, focus rings, transitions, skeleton loading, prefers-reduced-motion, advanced card variants |
| `anti-patterns.md` | 12 common AI mistakes with wrong code → correct code examples, dark mode surfaces, motion accessibility |

## Installation

There are several ways to install these skills in Claude Code:

### Option 1: Register as a Marketplace (recommended)

Add this repository as a plugin marketplace so Claude Code can discover and manage the skills:

```bash
# Inside Claude Code, run:
/plugin marketplace add MarkBenz/claude-code-skills
```

Then install the skill:

```bash
/plugin install tailwind-ui-rules@MarkBenz-claude-code-skills
```

To share with your team, install at project scope:

```bash
/plugin install tailwind-ui-rules@MarkBenz-claude-code-skills --scope project
```

### Option 2: Settings Configuration

Add the marketplace to your Claude Code settings file (`~/.claude/settings.json` for personal use, or `.claude/settings.json` in your project for team use):

```json
{
  "extraKnownMarketplaces": {
    "MarkBenz-claude-code-skills": {
      "source": {
        "source": "github",
        "repo": "MarkBenz/claude-code-skills"
      }
    }
  },
  "enabledPlugins": {
    "tailwind-ui-rules@MarkBenz-claude-code-skills": true
  }
}
```

### Option 3: Local Development / Testing

For temporary use in a single session (e.g., during development):

```bash
# Clone the repo
git clone https://github.com/MarkBenz/claude-code-skills.git

# Run Claude Code with the plugin directory
claude --plugin-dir ./claude-code-skills
```

### Verification

Once installed, the `tailwind-ui-rules` skill activates automatically whenever you work on frontend/UI tasks. Test it by asking Claude Code something like:

> "Build a card component with TailwindCSS"

The skill should load alongside the Anthropic `frontend-design` skill.

## Contributing

Feel free to open issues or PRs to suggest new rules, patterns, or additional skills.

## License

MIT
