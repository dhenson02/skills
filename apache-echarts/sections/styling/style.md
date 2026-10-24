# Style: Theme, Palette, Explicit Styles, Emphasis

Approaches overlap; pick by scope:

| Scope | Approach |
| --- | --- |
| Whole chart look | Theme |
| Series/data colors | Palette (`color`) |
| Specific elements | `itemStyle`, `lineStyle`, `areaStyle`, `label`, … |
| Color/size by value | [visualMap](../components/visual-map.md) |

## Theme

Built-in: `'light'` (default), `'dark'`.

```js
const chart = echarts.init(dom, 'dark');
```

Custom themes: create/download at the [theme builder](https://echarts.apache.org/theme-builder.html).

```js
// JSON theme: register manually
fetch('theme/vintage.json')
  .then((r) => r.json())
  .then((theme) => {
    echarts.registerTheme('vintage', theme);
    const chart = echarts.init(dom, 'vintage');
  });

// JS theme: self-registers once the script is loaded
const chart = echarts.init(dom, 'vintage');
```

Changing theme on a live chart: `chart.setTheme('vintage')` (v6+), or dispose and re-init.

## Palette

Colors are assigned in order to series (or to data items for pie etc.). Global or per-series:

```js
option = {
  color: ['#c23531', '#2f4554', '#61a0a8', '#d48265', '#91c7ae', '#749f83', '#ca8622'],  // global
  series: [
    { type: 'bar', color: ['#dd6b66', '#759aa0', '#e69d87'] },   // this series only
    { type: 'pie', color: ['#37A2DA', '#32C5E9', '#67E0E3'] }
  ]
};
```

## Explicit styles

Naming is consistent across series/components: `itemStyle`, `lineStyle`, `areaStyle`, `label`.

Colors accept CSS strings or gradients:

```js
// Linear: x, y, x2, y2 in 0–1 of the bounding box
color: { type: 'linear', x: 0, y: 0, x2: 0, y2: 1, colorStops: [{ offset: 0, color: 'red' }, { offset: 1, color: 'blue' }] }
// Radial: x, y, r in 0–1
color: { type: 'radial', x: 0.5, y: 0.5, r: 0.5, colorStops: [/* ... */] }
// Helper
color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{ offset: 0, color: '#83bff6' }, { offset: 1, color: '#188df0' }])
```

Bubble chart with shadow and radial gradient:

```js
option = {
  backgroundColor: {
    type: 'radial', x: 0.3, y: 0.3, r: 0.8,
    colorStops: [{ offset: 0, color: '#f7f8fa' }, { offset: 1, color: '#cdd0d5' }]
  },
  xAxis: { splitLine: { show: false } },
  yAxis: { splitLine: { show: false }, scale: true },
  series: [{
    name: '2015',
    type: 'scatter',
    data: [[44056, 81.8, 23968973, 'Australia'], [43294, 81.7, 35939927, 'Canada'], [13334, 76.9, 1376048943, 'China']],
    symbolSize: (d) => Math.sqrt(d[2]) / 5e2,
    emphasis: {
      focus: 'series',
      label: { show: true, formatter: (p) => p.data[3], position: 'top' }
    },
    itemStyle: {
      shadowBlur: 10,
      shadowColor: 'rgba(25, 100, 150, 0.5)',
      shadowOffsetY: 5,
      color: {
        type: 'radial', x: 0.4, y: 0.3, r: 1,
        colorStops: [{ offset: 0, color: 'rgb(129, 227, 238)' }, { offset: 1, color: 'rgb(25, 183, 207)' }]
      }
    }
  }]
};
```

## Emphasis (hover) state

Auto-derived from the normal style unless overridden in `emphasis` (same keys). Sibling states: `blur`, `select`.

```js
series: {
  type: 'scatter',
  itemStyle: { color: 'red' },
  label: { show: true, formatter: 'Normal label' },
  emphasis: {
    focus: 'series',                // 'self' | 'series' — blur the rest
    itemStyle: { color: 'blue' },
    label: { show: true, formatter: 'Emphasis label' }
  }
}
```

**Legacy (pre-v4) syntax** — `itemStyle: { normal: {...}, emphasis: {...} }` and `label: { normal: {...} }` — still parses but is deprecated. Write flat styles plus a sibling `emphasis` block.
