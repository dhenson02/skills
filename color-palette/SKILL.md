---
name: color-palette
description: "Build a UI color palette: greys, primary, and accent colors, each as a fixed 100–900 shade scale. Use when creating or extending a design system's colors, defining CSS color tokens/variables or a Tailwind theme, picking button/text/background/alert/state colors, or when a palette has too few shades or ad-hoc lighten()/darken() variants."
---

# Color Palette

Method from Refactoring UI. A usable palette is ~8–10 colors × 5–10 shades, not 5 hex codes from a generator.

## Rules

- Three categories: **greys** (8–10 shades), **primary** (1–2 colors, 5–10 shades), **accents** (semantic + highlight, multiple shades each).
- Define every shade up front as a fixed token. Never derive shades at runtime (`lighten()`, `darken()`, `color-mix()` ad hoc).
- Use a 9-step scale: `100` (lightest) … `500` (base) … `900` (darkest).
- No true black; darkest grey is a very dark grey.
- Pick by eye, not formula. No "50% lightness" rule — hues behave differently.
- Tweak shades after use in real UI; avoid adding *new* shades.

## Workflow

1. List needed colors: greys, primary, semantic states (danger, warning, success), highlight accents, categorical colors (charts, tags, calendars).
2. Per color, build a scale → [sections/shade-scales.md](sections/shade-scales.md).
3. Emit as tokens (CSS custom properties, Tailwind theme, etc.):

```css
:root {
  --color-primary-100: #f0f4fe;
  --color-primary-500: #6175de;
  --color-primary-900: #1f2c6d;
  /* ...all 9 steps per color */
}
```

4. Apply in real components; adjust saturation/lightness of existing shades as needed.

## Sections

- [sections/palette-composition.md](sections/palette-composition.md) — what colors a palette needs and where each is used
- [sections/shade-scales.md](sections/shade-scales.md) — building a 100–900 scale, with reference scales
- [sections/troubleshooting.md](sections/troubleshooting.md) — common failure modes
