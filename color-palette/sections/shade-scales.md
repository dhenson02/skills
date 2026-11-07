# Building a Shade Scale

9 steps: `100` lightest, `500` base, `900` darkest. Odd count makes midpoint subdivision clean.

## Steps

1. **Base (`500`)** — primary/accent: a shade that works as a button background. Choose by eye.
2. **Edges** — choose from real usage context:
   - `900`: text color (e.g. alert title on tinted background).
   - `100`: tinted background (e.g. alert background).
   - Start at the base hue; adjust saturation and lightness.
   - An alert component tests both at once: `100` background, `900` heading, mid-dark shade for body text.
3. **Midpoints** — pick `700` (between 500/900) and `300` (between 100/500). Each should look like the ideal compromise of its neighbors.
4. **Remaining gaps** — fill `800`, `600`, `400`, `200` the same way.
5. **Verify** in real components; tweak individual shades, don't add new ones.

## Greys

Same process; base matters less. Start from edges:

- `900`: darkest text color.
- `100`: subtle off-white background.

## Reference scales

Sampled from the source examples. Use as calibration, not templates.

| Step | Primary (blue) | Grey (cool) |
|---|---|---|
| 900 | `#1f2c6d` | `#202833` |
| 800 | `#253586` | `#404b5a` |
| 700 | `#3547a4` | `#6e7a8a` |
| 600 | `#495dc6` | `#929fb1` |
| 500 | `#6175de` | `#aebecd` |
| 400 | `#758ce0` | `#ccd4db` |
| 300 | `#95aeed` | `#d5dde5` |
| 200 | `#d4def8` | `#e1e7ec` |
| 100 | `#f0f4fe` | `#f8f9fa` |

Observations:
- Steps are not uniform in lightness; the light end jumps (300→200) more than the dark end.
- Darkest shades lower lightness *and* keep saturation, avoiding muddy near-black.
- Greys carry a slight cool (blue) tint rather than pure neutral.
