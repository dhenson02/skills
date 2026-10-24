# Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Blank chart, no error | Container has 0 height/width at `init` (hidden tab, unstyled div, flex child) | Give the container explicit size before `init`; call `resize()` when it becomes visible |
| Blank or missing parts with tree-shaken import | Chart/component/renderer not registered | Add to `echarts.use([...])` — renderer is mandatory; `dataset.transform` needs `TransformComponent`; `aria` needs `AriaComponent` ([setup/installation.md](setup/installation.md)) |
| Chart doesn't follow container size | No resize handling | `ResizeObserver` → `chart.resize()` ([setup/container-and-sizing.md](setup/container-and-sizing.md)) |
| Chart disappears after tab/route switch | DOM node replaced; instance bound to old node | `dispose()` on removal, `init` on re-mount |
| "There is a chart instance already initialized on the dom" | Double `init` on same node (e.g. React StrictMode) | `echarts.getInstanceByDom(dom) ?? echarts.init(dom)`; dispose in cleanup |
| Old series linger after update | `setOption` merges | `setOption(option, { notMerge: true })` or `{ replaceMerge: ['series'] }` |
| Wrong series updated | Merge by index | Give series stable `id` or `name` |
| `convertToPixel` returns `undefined` / throws | Called before first `setOption` | Call after the option with the coordinate system is set |
| `resize(800, 400)` ignored | Wrong signature | `resize({ width: 800, height: 400 })` |
| Line drawn through a missing point | `0` used for missing | Use `'-'` or `null` |
| `encode` has no effect | Dimension name typo | Match `dataset` dimension names exactly |
| Category labels skipped | Auto label interval | `axisLabel: { interval: 0, rotate: 30 }` |
| Value axis doesn't start at 0 / always starts at 0 | `scale` setting | `yAxis.scale: true` to fit data; omit to include 0 |
| Doughnut inner/outer swapped at some sizes | Mixed `%` and px radii | Use one unit for both |
| `barGap` / `barCategoryGap` ignored | Set on non-last bar series | Set on the last bar series in the grid |
| `getDataURL()` captures half-drawn chart | Animation still running | Wait for `finished` event, or `animation: false` |
| `aria` has no effect | Missing `AriaComponent` or used `aria.show` | Register component; set `aria.enabled: true` |
| SVG base map region not interactive | Element lacks `name` attribute | Add `name="..."` in the SVG ([charts/geo-map.md](charts/geo-map.md)) |
| Map series empty | Map not registered or names mismatch | `echarts.registerMap(name, geoJson)` before `setOption`; `data[].name` must equal feature `properties.name` |
| Slow with large data | Animation, per-item styles, SVG renderer | `animation: false`, `large: true`, `sampling: 'lttb'` (line), canvas renderer, `progressive` |
| Tooltip shows raw HTML or breaks layout | Unescaped data in HTML formatter | `echarts.format.encodeHTML()` ([best-practices/security.md](best-practices/security.md)) |
| Options like `itemStyle.normal` behave oddly | Deprecated v3 syntax | Flatten; use sibling `emphasis` |
