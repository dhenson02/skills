# Scatter / Bubble Chart

`series.type: 'scatter'`. Options: [series-scatter](https://echarts.apache.org/option.html#series-scatter). Extra dimensions (size, color) via `symbolSize` callback or [visualMap](../components/visual-map.md).

## Category x-axis

```js
option = {
  xAxis: { data: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'] },
  yAxis: {},
  series: [{ type: 'scatter', data: [220, 182, 191, 234, 290, 330, 310] }]
};
```

## Two value axes (typical)

```js
option = {
  xAxis: {},
  yAxis: {},
  series: [{ type: 'scatter', data: [[10, 5], [0, 8], [6, 10], [2, 12], [8, 9]] }]
};
```

## Symbol shape

[`symbol`](https://echarts.apache.org/option.html#series-scatter.symbol):

- Built-in: `'circle'`, `'rect'`, `'roundRect'`, `'triangle'`, `'diamond'`, `'pin'`, `'arrow'`, `'none'`.
- Image: `'image://https://example.com/x.png'` or `'image://./x.png'`.
- SVG path: `'path://'` + the `d` attribute of an SVG `<path>` (crisp at any size).

```js
series: [{
  type: 'scatter',
  data: [220, 182, 191, 234, 290, 330, 310],
  symbolSize: 20,
  // heart
  symbol: 'path://M51.911,16.242C51.152,7.888,45.239,1.827,37.839,1.827c-4.93,0-9.444,2.653-11.984,6.905 c-2.517-4.307-6.846-6.906-11.697-6.906c-7.399,0-13.313,6.061-14.071,14.415c-0.06,0.369-0.306,2.311,0.442,5.478 c1.078,4.568,3.568,8.723,7.199,12.013l18.115,16.439l18.426-16.438c3.631-3.291,6.121-7.445,7.199-12.014 C52.216,18.553,51.97,16.611,51.911,16.242z'
}]
```

## Symbol size

[`symbolSize`](https://echarts.apache.org/option.html#series-scatter.symbolSize): number, `[width, height]`, or callback `(value, params) => number | [w, h]`.

```js
series: [{
  type: 'scatter',
  data: [220, 182, 191, 234, 290, 330, 310],
  symbolSize: (value) => value / 10
}]
```

Bubble chart: data `[x, y, size, label]`, size from the third dimension:

```js
series: [{
  type: 'scatter',
  data: [[28604, 77, 17096869, 'Australia'], [31163, 77.4, 27662440, 'Canada']],
  symbolSize: (d) => Math.sqrt(d[2]) / 5e2
}]
```

For > ~2k points set `large: true` (batch rendering; disables some per-item effects).
