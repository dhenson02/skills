# SVG Base Map: Layout, Plotting, Limitations

Basics: [geo-map.md](geo-map.md). Layout rules apply equally to `geo` and `series-map`.

## Layout

Default: centered in the canvas. Usually enough to adjust `layoutCenter` / `layoutSize`; occasionally `<svg viewBox>` or `geo.boundingCoords` (difference: whether content outside is clipped).

Three steps determine placement:

1. **Shape** — from the SVG itself (`viewBox` clips).
2. **Viewport** (canvas px or %), one of:
   - `layoutCenter` + `layoutSize` (recommended) — bounding rect fitted inside, aspect ratio kept.
   - `top` / `right` / `bottom` / `left` — bounding rect stretched to fill.
3. **Bounding rect** (SVG units), first match wins:
   1. `geo.boundingCoords`
   2. `<svg width height>` → `[0, 0, width, height]`
   3. `<svg viewBox>`
   4. Union of all element bounds

   Then transformed by `geo.center` / `geo.zoom` if set.

## Plotting series on an SVG map

`scatter`, `effectScatter`, `lines`, `custom` accept `coordinateSystem: 'geo'`. Data values are **SVG local coordinates**, not lng/lat.

```js
option = {
  geo: { map: 'some_svg' },
  series: {
    type: 'effectScatter',
    coordinateSystem: 'geo',
    geoIndex: 0,
    data: [[488.2, 459.7], [770.3, 757.9], [1180.0, 743.6]]
  }
};
```

Find coordinates by clicking:

```js
chart.getZr().on('click', (e) => {
  console.log(chart.convertFromPixel({ geoIndex: 0 }, [e.offsetX, e.offsetY]));
});
```

## Unsupported SVG features

- `transform: skew(...)` (or matrices containing skew); `scale(x, y)` with opposite signs combined with `rotate` (fixed in v5.1.2+).
- Standalone `<style>` tags — inline `style="..."` works.
- Units other than `px`; percentage sizes like `<svg width="30%">`.
- `<defs>` contents other than `<linearGradient>` / `<radialGradient>`; gradients ignore `fx`, `fy`, `gradientTransform`.
- `fill`/`stroke` `url(...)` other than `url(#id)`.
- `<switch>` — all children render.
- `<text>`: `textPath`; per-character positions (`<tspan x="0 4.94 9.89">abc</tspan>`) — split into one `<tspan>` per character instead.
