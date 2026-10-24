# Pie, Doughnut, Rose

`series.type: 'pie'`. No axes. Options: [series-pie](https://echarts.apache.org/option.html#series-pie).

## Basic

Values are raw; ECharts computes proportions.

```js
option = {
  series: [{
    type: 'pie',
    radius: '50%',     // % of the container's shorter side, or px number
    data: [
      { value: 335, name: 'Direct Visit' },
      { value: 234, name: 'Union Ad' },
      { value: 1548, name: 'Search Engine' }
    ]
  }]
};
```

Position with `center: ['50%', '50%']`.

## All-zero data

By default an all-zero pie is drawn as equal slices. Hide it:

```js
series: [{
  type: 'pie',
  stillShowZeroSum: false,
  label: { show: false },    // also hide labels
  data: [{ value: 0, name: 'A' }, { value: 0, name: 'B' }]
}]
```

## Doughnut

`radius: [inner, outer]`. Mixing `%` and px can invert them at some sizes (ECharts uses the smaller as inner) — use the same unit for both.

```js
option = {
  title: { text: 'Doughnut', left: 'center', top: 'center' },   // fixed center text
  series: [{
    type: 'pie',
    radius: ['40%', '70%'],
    data: [
      { value: 335, name: 'A' },
      { value: 234, name: 'B' },
      { value: 1548, name: 'C' }
    ]
  }]
};
```

### Center text from hovered slice

Hide labels, pin them to the center, show only on emphasis. `avoidLabelOverlap: false` keeps the label centered.

```js
option = {
  legend: { orient: 'vertical', left: 'left' },
  series: [{
    type: 'pie',
    radius: ['50%', '70%'],
    avoidLabelOverlap: false,
    label: { show: false, position: 'center' },
    labelLine: { show: false },
    emphasis: { label: { show: true, fontSize: 30, fontWeight: 'bold' } },
    data: [
      { value: 335, name: 'A' },
      { value: 310, name: 'B' },
      { value: 234, name: 'C' },
      { value: 135, name: 'D' },
      { value: 1548, name: 'E' }
    ]
  }]
};
```

## Rose (Nightingale)

[`roseType`](https://echarts.apache.org/option.html#series-pie.roseType): `'area'` — equal angles, radius encodes value; `'radius'` — both angle and radius encode value.

```js
series: [{
  type: 'pie',
  roseType: 'area',
  data: [
    { value: 100, name: 'A' },
    { value: 200, name: 'B' },
    { value: 300, name: 'C' },
    { value: 400, name: 'D' },
    { value: 500, name: 'E' }
  ]
}]
```
