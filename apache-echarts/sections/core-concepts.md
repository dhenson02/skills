# Core Concepts

## The option object

A chart is fully described by one declarative `option` object passed to `chart.setOption(option)`. Top-level keys are **components** (most accept an object or an array for multiples):

| Key | Role |
| --- | --- |
| `series` | Array of data series; `type` picks the chart (`'bar'`, `'line'`, `'pie'`, `'scatter'`, `'map'`, `'custom'`, …) |
| `dataset` | Tabular data shared by series ([data/_index.md](data/_index.md)) |
| `xAxis`, `yAxis`, `grid` | Cartesian coordinate system |
| `polar`, `angleAxis`, `radiusAxis` / `geo` / `singleAxis` / `calendar` / `parallel` | Other coordinate systems |
| `title`, `legend`, `tooltip`, `toolbox` | Chrome |
| `dataZoom`, `visualMap`, `brush`, `timeline` | Interactive components |
| `graphic` | Free-form shapes/text/images |
| `color`, `backgroundColor`, `textStyle`, `animation*` | Global style |
| `aria` | Accessibility |

Series choose a coordinate system with `coordinateSystem` (`'cartesian2d'` default for bar/line/scatter) and bind to specific components via `xAxisIndex`, `yAxisIndex`, `gridIndex`, `polarIndex`, `geoIndex`, `datasetIndex`.

Full option reference: <https://echarts.apache.org/option.html>. API reference: <https://echarts.apache.org/api.html>. Examples gallery: <https://echarts.apache.org/examples>.

## Data item shapes

```js
data: [5, 20, 36]                                        // one value per category
data: [[10, 5], [0, 8]]                                  // [x, y, ...extra dims]
data: [{ name: 'A', value: 335, itemStyle: { color: 'red' } }]   // object: per-item options
data: [0, 22, '-', 23]                                   // '-' or null = missing
```

## Tooltip

```js
tooltip: { trigger: 'axis' }   // all series at the hovered x (line/bar)
tooltip: { trigger: 'item' }   // single item (pie/scatter/map) — default
tooltip: { trigger: 'axis', axisPointer: { type: 'cross' } }
tooltip: { formatter: '{b}: {c}' }                       // template: {a} series, {b} name, {c} value, {d} percent
tooltip: { valueFormatter: (v) => v.toFixed(1) + ' ms' } // format values, keep default layout
```

HTML-returning formatter callbacks must escape data — see [best-practices/security.md](best-practices/security.md).

## Positioning components

`left`/`right`/`top`/`bottom` accept px numbers, `'20%'`, or `'left' | 'center' | 'right'` / `'top' | 'middle' | 'bottom'`. `grid` margins control the plot area; `grid.containLabel: true` includes axis labels inside the margins.

## Instance lifecycle

```js
const chart = echarts.init(dom, theme, opts);   // theme: null | 'dark' | name; opts: { renderer, width, height, locale }
chart.setOption(option);                  // render / update (merge)
chart.setOption(partial, { notMerge });   // replace instead of merge
chart.resize();                           // after container size change
chart.on('click', handler);               // events
chart.dispatchAction({ type: 'highlight', seriesIndex: 0, dataIndex: 1 });
chart.showLoading(); chart.hideLoading();
chart.getDataURL({ type: 'png', pixelRatio: 2 });   // export image
chart.clear();                            // remove all components/series
chart.dispose();                          // destroy; required before removing the DOM node
```

### Framework wrapper pattern (React/Vue/Svelte)

Init once on mount, update via `setOption` when data changes, observe size, dispose on unmount:

```js
// React
useEffect(() => {
  const chart = echarts.init(ref.current);
  const observer = new ResizeObserver(() => chart.resize());
  observer.observe(ref.current);
  chartRef.current = chart;
  return () => {
    observer.disconnect();
    chart.dispose();
  };
}, []);

useEffect(() => {
  chartRef.current?.setOption(option, { notMerge: true });
}, [option]);
```

## Update semantics

`setOption` merges by default: components/series match by `id`, then `name`, then index. Data diffs by item `name` to drive transitions ([styling/animation.md](styling/animation.md)). Removing series requires `notMerge: true` or `replaceMerge: ['series']` ([data/async-and-updates.md](data/async-and-updates.md)).

## Version notes

- v4: `dataset`, SVG renderer.
- v5: data transforms, bar race, decals, `emphasis`/`blur`/`select` states; tree-shaken `echarts/core` imports; `aria` needs `AriaComponent`.
- v5.3: SSR SVG string rendering. v5.4: pointer snapping. v5.5: SSR light client runtime.
- v6: registerable custom series, `setTheme`.
- Legacy `itemStyle.normal` / `label.normal` wrappers: deprecated since v4 — don't emit them.
