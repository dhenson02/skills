# Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| UI looks garish, color everywhere | Built from a 5-swatch generator (triad, complementary, etc.) | Make most surfaces grey; reserve primary for actions/emphasis |
| "Need something between grey 2 and 3" | Too few greys | Expand to 8–10 shades |
| Dozens of near-identical blues in codebase | Runtime `lighten()`/`darken()` | Replace with fixed 100–900 tokens |
| Dark text looks harsh/unnatural | True black `#000` | Use very dark grey as darkest shade |
| Shade fails in context | Chosen by numeric formula | Adjust saturation/lightness by eye in the real component |
| Palette sprawl, no consistency | Adding new shades instead of tweaking | Edit existing shade values; add only when truly unavoidable |
| Accents dominate | Overused | Limit accents to semantic states and targeted highlights |
