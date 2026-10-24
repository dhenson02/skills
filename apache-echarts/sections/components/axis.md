# Cartesian Axes

Options: [xAxis](https://echarts.apache.org/option.html#xAxis), [yAxis](https://echarts.apache.org/option.html#yAxis). Axes live in a [`grid`](https://echarts.apache.org/option.html#grid) (plot area; set margins with `left`/`right`/`top`/`bottom`).

Anatomy: axis line, ticks, tick labels, axis name (title), and optional split lines (gridlines).

## Types

| `type` | Data | Typical use |
| --- | --- | --- |
| `'value'` | Numbers (default) | Measures |
| `'category'` | Discrete labels from `axis.data` or dataset | Products, weekdays |
| `'time'` | Timestamps / date strings | Time series |
| `'log'` | Positive numbers | Wide ranges |

```js
option = {
  xAxis: { type: 'time', name: 'Sales Time' },
  yAxis: { type: 'value', name: 'Sales Quantity' },
  dataZoom: [{ type: 'inside' }, { type: 'slider' }]   // zoom long x ranges
};
```

Useful: `min`/`max` (number, `'dataMin'`, `'dataMax'`, or function), `scale: true` (value axis not forced to include 0), `inverse`, `boundaryGap` (category padding), `position`, `offset`, `splitLine: { show }`.

## Multiple axes

Array form; series choose via `xAxisIndex` / `yAxisIndex`. Use `position` (`'left' | 'right'`, `'top' | 'bottom'`) and [`offset`](https://echarts.apache.org/option.html#xAxis.offset) to avoid overlap.

## Axis line, ticks, labels

```js
xAxis: {
  axisLine: { symbol: 'arrow', lineStyle: { type: 'dashed' } },          // axisLine.onZero: false to detach from 0
  axisTick: { length: 6, alignWithLabel: true, lineStyle: { type: 'dashed' } },
  axisLabel: {
    formatter: '{value} kg',     // string template or (value, index) => string
    rotate: 30,                  // long category labels
    interval: 0,                 // show every label (0) / auto (default)
    align: 'center'
  }
}
```

## Example: dual y-axes (bar + line)

```js
option = {
  tooltip: { trigger: 'axis', axisPointer: { type: 'cross' } },
  legend: {},
  xAxis: [{
    type: 'category',
    axisTick: { alignWithLabel: true },
    axisLabel: { rotate: 30 },
    data: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
  }],
  yAxis: [
    { type: 'value', name: 'Precipitation', min: 0, max: 250, position: 'right', axisLabel: { formatter: '{value} ml' } },
    { type: 'value', name: 'Temperature', min: 0, max: 25, position: 'left', axisLabel: { formatter: '{value} °C' } }
  ],
  series: [
    {
      name: 'Precipitation',
      type: 'bar',
      yAxisIndex: 0,
      data: [6, 32, 70, 86, 68.7, 100.7, 125.6, 112.2, 78.7, 48.8, 36.0, 19.3]
    },
    {
      name: 'Temperature',
      type: 'line',
      smooth: true,
      yAxisIndex: 1,
      data: [6.0, 10.2, 10.3, 11.5, 10.3, 13.2, 14.3, 16.4, 18.0, 16.5, 12.0, 5.2]
    }
  ]
};
```
