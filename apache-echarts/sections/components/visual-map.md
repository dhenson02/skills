# visualMap: Data → Color/Size

[`visualMap`](https://echarts.apache.org/option.html#visualMap) maps a data **dimension** to visual channels. Use it for heatmaps, choropleths, bubble size, or coloring by a third variable.

Channels: `symbol`, `symbolSize`, `color`, `opacity`, `colorAlpha`, `colorLightness`, `colorSaturation`, `colorHue`.

## Dimensions

Each data item's value is a number (one dimension) or an array (multiple):

```js
series: {
  data: [
    { value: [3434, 129, 'San Marino'], itemStyle: {} },   // object form
    [1212, 5454, 'Vatican'],                               // array form (common)
    [2323, 3223, 'Nauru']
  ]
}
```

Typically dims 0/1 → x/y; visualMap encodes a further dim (`dimension`, default: last dimension).

## Continuous

```js
visualMap: {
  type: 'continuous',
  min: 0,
  max: 5000,
  dimension: 3,            // value[3]
  seriesIndex: 4,          // only series index 4 (default: all)
  inRange: {
    color: ['blue', '#121122', 'red'],   // min → max, interpolated
    symbolSize: [30, 100]
  },
  outOfRange: { symbolSize: [30, 100] }  // style for values outside the selected range
}
```

## Piecewise

`type: 'piecewise'` with one of:

- [`splitNumber`](https://echarts.apache.org/option.html#visualMap-piecewise.splitNumber) — equal segments.
- [`pieces`](https://echarts.apache.org/option.html#visualMap-piecewise.pieces) — custom ranges.
- [`categories`](https://echarts.apache.org/option.html#visualMap-piecewise.categories) — discrete values.

```js
visualMap: {
  type: 'piecewise',
  dimension: 2,
  pieces: [
    { min: 1500, color: '#d94e5d' },
    { min: 900, max: 1500, color: '#eac736' },
    { max: 900, color: '#50a3ba' }
  ]
}
```

Multiple visualMaps: pass an array. `show: false` hides the control while keeping the mapping.
