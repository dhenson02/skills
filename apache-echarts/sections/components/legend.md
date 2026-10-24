# Legend

Options: [legend](https://echarts.apache.org/option.html#legend). Items come from series names (or data names for pie); `legend.data` is optional. Clicking toggles visibility.

Guidance:

- Single-series chart → use a title instead of a legend.
- Keep legend placement consistent across charts on a page.
- Crowded or short chart → place at the bottom.
- Mixed chart types (e.g. bar + line) → distinct `icon` per item.

## Layout

```js
option = {
  legend: {
    orient: 'vertical',      // or 'horizontal' (default)
    right: 10,
    top: 'center'
  },
  dataset: {
    source: [
      ['product', '2015', '2016', '2017'],
      ['Matcha Latte', 43.3, 85.8, 93.7],
      ['Milk Tea', 83.1, 73.4, 55.1]
    ]
  },
  xAxis: { type: 'category' },
  yAxis: {},
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]
};
```

Many items → scrollable:

```js
legend: { type: 'scroll', orient: 'vertical', right: 10, top: 20, bottom: 20 }
```

## Style

```js
legend: {
  icon: 'rect',                 // 'circle' | 'rect' | 'roundRect' | 'triangle' | 'diamond' | 'pin' | 'arrow' | 'none' | 'path://…'
  backgroundColor: 'rgba(255,255,255,0.2)',   // translucent on dark backgrounds
  textStyle: { color: '#ccc' }
}
```

Default icons follow series type (e.g. line series show a line with a marker; bar series a rect).

## Initial selection

```js
legend: {
  selected: { 'Legend A': true, 'Legend B': true, 'Legend C': false }   // C hidden initially
}
```

`selectedMode: false` disables toggling; `'single'` allows one at a time.

## Per-item icons

```js
legend: {
  data: [
    { name: 'Legend A', icon: 'rect' },
    { name: 'Legend B', icon: 'circle' },
    { name: 'Legend C', icon: 'pin' }
  ]
},
series: [{ name: 'Legend A' /* ... */ }, { name: 'Legend B' /* ... */ }, { name: 'Legend C' /* ... */ }]
```

Legend toggles emit `legendselectchanged` — see [events-and-actions.md](../interaction/events-and-actions.md).
