# Line Chart

`series.type: 'line'`. Options: [series-line](https://echarts.apache.org/option.html#series-line).

## Category x-axis

```js
option = {
  xAxis: { type: 'category', data: ['A', 'B', 'C'] },
  yAxis: { type: 'value' },
  series: [{ type: 'line', data: [120, 200, 150] }]
};
```

`type` may be omitted: axes default to `'value'`, and an axis with `data` is inferred as `'category'`.

## Numeric x-axis

Each point is `[x, y]`; both axes are value axes.

```js
option = {
  xAxis: {},
  yAxis: {},
  series: [{ type: 'line', data: [[20, 120], [50, 200], [40, 50]] }]
};
```

## Styling

- [`lineStyle`](https://echarts.apache.org/option.html#series-line.lineStyle): `color`, `width`, `type` (`'solid' | 'dashed' | 'dotted'`), `opacity`, shadows.
- [`itemStyle`](https://echarts.apache.org/option.html#series-line.itemStyle): point markers (`color`, `borderColor`, `borderWidth`, `opacity`, …) — independent of `lineStyle`.
- `symbol`, `symbolSize`, `showSymbol: false` to hide markers.

```js
series: [{
  type: 'line',
  data: [10, 22, 28, 23, 19],
  lineStyle: { color: 'green', width: 4, type: 'dashed' }
}]
```

## Value labels

`label.show: true` shows values always; `emphasis.label.show: true` shows on hover only.

```js
series: [{
  type: 'line',
  data: [10, 22, 28, 23, 19],
  label: { show: true, position: 'bottom', fontSize: 20 }
}]
```

## Missing data

Use `'-'` (or `null`) for a gap — the line breaks there. `0` is a real value and is plotted. `connectNulls: true` bridges gaps.

```js
series: [{ type: 'line', data: [0, 22, '-', 23, 19] }]
```

## Variants

```js
// Stacked — same `stack` key; add areaStyle so stacking is visually obvious
series: [
  { type: 'line', data: [10, 22, 28, 43, 49], stack: 'x', areaStyle: {} },
  { type: 'line', data: [5, 4, 3, 5, 10], stack: 'x', areaStyle: {} }
]

// Area — {} fills with a translucent series color; override as needed
series: [
  { type: 'line', data: [10, 22, 28, 23, 19], areaStyle: {} },
  { type: 'line', data: [25, 14, 23, 35, 10], areaStyle: { color: '#ff0', opacity: 0.5 } }
]

// Smooth
series: [{ type: 'line', data: [10, 22, 28, 23, 19], smooth: true }]

// Step — corner at 'start' (point A), 'middle', or 'end' (point B)
series: [
  { name: 'Step Start', type: 'line', step: 'start', data: [120, 132, 101, 134, 90, 230, 210] },
  { name: 'Step Middle', type: 'line', step: 'middle', data: [220, 282, 201, 234, 290, 430, 410] },
  { name: 'Step End', type: 'line', step: 'end', data: [450, 432, 401, 454, 590, 530, 510] }
]
```
