# Dataset Mapping: Dimensions, encode, seriesLayoutBy

Prerequisite: [dataset.md](dataset.md).

## Contents
- Rows vs columns (`seriesLayoutBy`)
- Dimensions and types
- `series.encode`
- Default mapping rules
- Recipes

## Rows vs columns

[`series.seriesLayoutBy`](https://echarts.apache.org/option.html#series.seriesLayoutBy):

- `'column'` (default) — each series takes a column.
- `'row'` — each series takes a row.

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    source: [
      ['product', '2012', '2013', '2014', '2015'],
      ['Matcha Latte', 41.1, 30.4, 65.1, 53.3],
      ['Milk Tea', 86.5, 92.1, 85.7, 83.1],
      ['Cheese Cocoa', 24.1, 67.2, 79.5, 86.4]
    ]
  },
  xAxis: [{ type: 'category', gridIndex: 0 }, { type: 'category', gridIndex: 1 }],
  yAxis: [{ gridIndex: 0 }, { gridIndex: 1 }],
  grid: [{ bottom: '55%' }, { top: '55%' }],
  series: [
    // Top grid: one series per row
    { type: 'bar', seriesLayoutBy: 'row', xAxisIndex: 0, yAxisIndex: 0 },
    { type: 'bar', seriesLayoutBy: 'row', xAxisIndex: 0, yAxisIndex: 0 },
    { type: 'bar', seriesLayoutBy: 'row', xAxisIndex: 0, yAxisIndex: 0 },
    // Bottom grid: one series per column
    { type: 'bar', seriesLayoutBy: 'column', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', seriesLayoutBy: 'column', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', seriesLayoutBy: 'column', xAxisIndex: 1, yAxisIndex: 1 },
    { type: 'bar', seriesLayoutBy: 'column', xAxisIndex: 1, yAxisIndex: 1 }
  ]
};
```

## Dimensions

When a series maps to columns, each column is a **dimension** and each row an **item** (swapped for `seriesLayoutBy: 'row'`).

Dimension names come from the first row/column (auto-detected; force with `dataset.sourceHeader: true`) or are declared explicitly — declaring also allows setting the type:

```js
dataset: {
  dimensions: [
    { name: 'score' },
    'amount',                              // shorthand: name only
    { name: 'product', type: 'ordinal' }
  ],
  source: [/* ... */]
}

// series.dimensions overrides dataset.dimensions
series: {
  type: 'line',
  dimensions: [null, 'amount', { name: 'product', type: 'ordinal' }]  // null = unnamed
}
```

Types (usually auto-detected; set manually if detection is wrong):

| Type | Use |
| --- | --- |
| `'number'` | Default numeric data |
| `'ordinal'` | Strings/categories; required for text on an axis |
| `'time'` | Parses values like `'2017-05-10'` as timestamps; implied when used on a `time` axis |
| `'float'` / `'int'` | Stored in `TypedArray` for performance |

## series.encode

[`series.encode`](https://echarts.apache.org/option.html#series.encode) maps dimensions (by name or 0-based index; arrays allowed) to targets.

```js
option = {
  dataset: {
    source: [
      ['score', 'amount', 'product'],
      [89.3, 58212, 'Matcha Latte'],
      [57.1, 78254, 'Milk Tea'],
      [74.4, 41032, 'Cheese Cocoa'],
      [19.6, 91852, 'Orange Juice']
    ]
  },
  xAxis: {},
  yAxis: { type: 'category' },
  series: [{ type: 'bar', encode: { x: 'amount', y: 'product' } }]
};
```

Targets:

```js
// Any series / coordinate system
encode: {
  tooltip: ['product', 'score'],  // dimensions shown in tooltip
  seriesName: [1, 3],             // joined into series name
  itemId: 2,                      // stable id for update animations on setOption
  itemName: 3                     // item name (e.g. pie legend)
}

encode: { x: [1, 5, 'score'], y: 0 }   // cartesian (grid)
encode: { single: 3 }                  // singleAxis
encode: { radius: 3, angle: 2 }        // polar
encode: { lng: 3, lat: 2 }             // geo
encode: { value: 3 }                   // no coordinate system (pie, funnel)
```

## Default mapping (no encode)

For line, bar, scatter, candlestick, etc.:

- With a category axis: first column → category axis; each following column → one series.
- With no category axis: each pair of columns → one series (x, y).
- Without axes (pie): first column → name, second → value.

Richer examples: [dataset-encode1](https://echarts.apache.org/examples/en/editor.html?c=dataset-encode1), [dataset-default](https://echarts.apache.org/examples/en/editor.html?c=dataset-default).

## Recipes

```js
// 3rd column → x, 5th column → y
series: { encode: { x: 2, y: 4 } }

// 3rd row → x, 5th row → y
series: { encode: { x: 2, y: 4 }, seriesLayoutBy: 'row' }

// Label text from dimensions: {@name} by name, {@[4]} by index
series: { label: { show: true, formatter: '{@product}: {@score} ({@[4]})' } }

// Show 2nd and 3rd columns in tooltip
series: { encode: { tooltip: [1, 2] } }

// Name dimensions when the source has no header row
dataset: { dimensions: ['score', 'amount'], source: [[89.3, 3371], [92.1, 8123]] }
```

Map a 3rd column to bubble size with `visualMap` (see [components/visual-map.md](../components/visual-map.md)):

```js
option = {
  dataset: { source: [[12, 323, 11.2], [23, 167, 8.3], [81, 284, 12], [91, 413, 4.1]] },
  visualMap: {
    show: false,
    dimension: 2,     // 3rd column
    min: 2,
    max: 15,
    inRange: { symbolSize: [5, 60] }
  },
  xAxis: {},
  yAxis: {},
  series: { type: 'scatter' }
};
```

**encode has no effect?** Check dimension-name spelling — a misspelled name fails silently.
