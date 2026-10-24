# Bar Chart

`series.type: 'bar'`. Options: [series-bar](https://echarts.apache.org/option.html#series-bar). Race and waterfall: [bar-advanced.md](bar-advanced.md).

## Basic and multi-series

```js
option = {
  xAxis: { data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'] },  // category axis
  yAxis: {},                                                            // value axis, auto range
  series: [
    { type: 'bar', data: [23, 24, 18, 25, 27, 28, 25] },
    { type: 'bar', data: [26, 24, 18, 22, 23, 20, 27] }   // second series → grouped bars
  ]
};
```

Horizontal bars: swap axes — `yAxis: { type: 'category', data: [...] }`, `xAxis: { type: 'value' }`.

## Item style

[`itemStyle`](https://echarts.apache.org/option.html#series-bar.itemStyle): `color`, `borderColor`, `borderWidth`, `borderType`, `borderRadius`, `opacity`, `shadowBlur`, `shadowColor`, `shadowOffsetX`, `shadowOffsetY`. Set per series or per data item.

```js
option = {
  xAxis: { data: ['A', 'B', 'C', 'D', 'E'] },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [
        10,
        22,
        28,
        { value: 43, itemStyle: { color: '#91cc75', borderType: 'dashed', opacity: 0.5 } },  // single bar
        49
      ],
      itemStyle: {
        borderRadius: 5,          // number or [tl, tr, br, bl]
        borderWidth: 1,
        borderType: 'solid',
        borderColor: '#73c0de',
        shadowColor: '#5470c6',
        shadowBlur: 3
      }
    }
  ]
};
```

## Width and spacing

| Option | Meaning |
| --- | --- |
| [`barWidth`](https://echarts.apache.org/option.html#series-bar.barWidth) | Absolute px or `%` of category width |
| [`barMaxWidth`](https://echarts.apache.org/option.html#series-bar.barMaxWidth) | Cap width on large charts |
| [`barMinHeight`](https://echarts.apache.org/option.html#series-bar.barMinHeight) | Minimum visible height for tiny values |
| [`barGap`](https://echarts.apache.org/option.html#series-bar.barGap) | Gap between bars of different series in one category, `%` of bar width |
| [`barCategoryGap`](https://echarts.apache.org/option.html#series-bar.barCategoryGap) | Gap between categories, `%` of category width |

```js
series: [
  { type: 'bar', data: [23, 24, 18, 25, 18], barGap: '20%', barCategoryGap: '40%' },
  { type: 'bar', data: [12, 14, 9, 9, 11] }
]
```

- With `barGap` + `barCategoryGap` set, `barWidth` is usually unnecessary; add `barMaxWidth` to prevent fat bars.
- `barGap`/`barCategoryGap` are shared by all bar series in one grid — set them on the **last** bar series.

## Background

```js
series: [{
  type: 'bar',
  data: [120, 200, 150, 80, 70, 110, 130],
  showBackground: true,
  backgroundStyle: { color: 'rgba(220, 220, 220, 0.8)' }
}]
```

## Stacked

Series sharing the same `stack` string stack together. Use meaningful names (`'male'`, `'female'`) when there are several stack groups.

```js
option = {
  xAxis: { data: ['A', 'B', 'C', 'D', 'E'] },
  yAxis: {},
  series: [
    { type: 'bar', data: [10, 22, 28, 43, 49], stack: 'total' },
    { type: 'bar', data: [5, 4, 3, 5, 10], stack: 'total' }
  ]
};
```
