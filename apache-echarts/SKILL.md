---
name: apache-echarts
description: "Build, configure, style, and debug interactive charts with Apache ECharts (the `echarts` JavaScript library): bar, line, pie, scatter, maps, custom series, dashboards, server-side rendered SVG/PNG charts. Use whenever the user mentions ECharts, echarts-for-react/vue-echarts, `setOption`, or an ECharts `option` object, or needs a JavaScript/web chart and hasn't chosen another library."
---

# Apache ECharts

Declarative JS charting: build one `option` object, pass it to `chart.setOption()`. Targets ECharts 5/6.

## Quick start

```html
<script src="https://cdn.jsdelivr.net/npm/echarts/dist/echarts.min.js"></script>

<!-- Container MUST have a size before init -->
<div id="main" style="width: 600px; height: 400px;"></div>

<script>
  const chart = echarts.init(document.getElementById('main'));

  chart.setOption({
    title: { text: 'Sales' },
    tooltip: { trigger: 'axis' },
    legend: {},
    xAxis: { type: 'category', data: ['Shirts', 'Cardigans', 'Chiffons', 'Pants', 'Heels', 'Socks'] },
    yAxis: { type: 'value' },
    series: [{ name: 'sales', type: 'bar', data: [5, 20, 36, 10, 10, 20] }]
  });

  new ResizeObserver(() => chart.resize()).observe(document.getElementById('main'));
</script>
```

npm: `npm install echarts` → `import * as echarts from 'echarts'`. Tree-shaken imports: [sections/setup/installation.md](sections/setup/installation.md).

## Workflow

1. Pick the series `type` and coordinate system; read that chart's file under [sections/charts/](sections/charts/_index.md).
2. Put tabular data in `dataset` and map with `encode` if data is shared or column-oriented; otherwise inline `series.data`.
3. Add components: `tooltip`, `legend`, axes, `dataZoom`, `visualMap`.
4. Style via palette/theme first, explicit `itemStyle`/`label` second.
5. Handle lifecycle: size the container, `resize()` on change, `dispose()` on teardown.
6. Update with `setOption` (merges); use `{ notMerge: true }` or `replaceMerge` when removing series.

## Rules that prevent most bugs

- Container needs non-zero width/height at `init`. Blank chart → check this first.
- Tree-shaken builds must register every chart, component, and a renderer (`CanvasRenderer`/`SVGRenderer`) via `echarts.use`.
- Call `dispose()` before removing the DOM node; re-`init` on re-mount. Guard double-init with `echarts.getInstanceByDom`.
- Give series `name`/`id` so partial `setOption` updates target the right one.
- Missing values: `'-'` or `null`, not `0`.
- `convertToPixel`/`convertFromPixel` only work after the first `setOption`.
- Don't emit legacy `itemStyle: { normal: {...} }` syntax; write flat styles plus sibling `emphasis`.
- Escape data in HTML-returning `tooltip.formatter` with `echarts.format.encodeHTML`; never pass untrusted URLs, CSS, regexes, or function source into options.

## Sections

Read only what the task needs.

| Section | Contents |
| --- | --- |
| [core-concepts.md](sections/core-concepts.md) | Option model, components, data shapes, tooltip, positioning, instance API, framework wrapper, update semantics, versions |
| [setup/](sections/setup/_index.md) | Installation & imports, TypeScript types, container sizing/resize/dispose, canvas vs SVG |
| [data/](sections/data/_index.md) | `dataset`, `encode`/dimensions, transforms (filter/sort/ecStat), async loading, live updates |
| [charts/](sections/charts/_index.md) | Bar (+ race, waterfall), line/area, pie/doughnut/rose, scatter/bubble, custom series, GeoJSON & SVG maps |
| [components/](sections/components/_index.md) | Axes, legend, visualMap, labels, rich text |
| [styling/](sections/styling/_index.md) | Themes, palettes, gradients, emphasis state, animation |
| [interaction/](sections/interaction/_index.md) | Events, query filters, `dispatchAction`, draggable points, pointer snapping |
| [ssr/](sections/ssr/_index.md) | Node.js SVG/PNG rendering, hydration, strategy comparison |
| [best-practices/](sections/best-practices/_index.md) | Accessibility (aria, decals), security checklist |
| [troubleshooting.md](sections/troubleshooting.md) | Symptom → cause → fix table |

Authoritative references: [option](https://echarts.apache.org/option.html) · [API](https://echarts.apache.org/api.html) · [examples](https://echarts.apache.org/examples) (its "Full Code" tab generates tree-shaken imports).
