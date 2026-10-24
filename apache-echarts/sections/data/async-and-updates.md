# Async Loading and Dynamic Updates

All updates go through [`setOption`](https://echarts.apache.org/api.html#echartsInstance.setOption). By default it **merges** into the current option; ECharts diffs old vs new data and animates the change.

## Load, then render

```js
const chart = echarts.init(document.getElementById('main'));

fetch('data.json')
  .then((res) => res.json())
  .then((data) => {
    // data: { categories: ['Shirt', ...], values: [5, 20, ...] }
    chart.setOption({
      tooltip: {},
      xAxis: { data: data.categories },
      yAxis: {},
      series: [{ name: 'Sales', type: 'bar', data: data.values }]
    });
  });
```

## Render skeleton, fill data later

Set styles, axes, and empty series first; later `setOption` calls only need the changing parts.

```js
chart.setOption({
  tooltip: {},
  legend: { data: ['Sales'] },
  xAxis: { data: [] },
  yAxis: {},
  series: [{ name: 'Sales', type: 'bar', data: [] }]
});

fetch('data.json')
  .then((res) => res.json())
  .then((data) => {
    chart.setOption({
      xAxis: { data: data.categories },
      series: [{ name: 'Sales', data: data.values }]  // matched by name
    });
  });
```

Series merge by `id`, then `name`, then array index. Give series a `name` or `id` so partial updates hit the right one.

## Loading indicator

```js
chart.showLoading();
fetch('data.json')
  .then((res) => res.json())
  .then((data) => {
    chart.hideLoading();
    chart.setOption(/* ... */);
  });
```

## Periodic updates

Poll or push, then `setOption` with the new data. Transitions are automatic; tune them in [styling/animation.md](../styling/animation.md).

```js
setInterval(async () => {
  const data = await fetch('/latest').then((r) => r.json());
  chart.setOption({ series: [{ name: 'Sales', data: data.values }] });
}, 5000);
```

## Merge control

`chart.setOption(option, opts)`:

| opts | Effect |
| --- | --- |
| `notMerge: true` | Replace the whole option (removes stale series/components) |
| `replaceMerge: ['series']` | Replace only the listed component types; merge the rest |
| `lazyUpdate: true` | Defer rendering to the next frame |
| `silent: true` | Suppress events from this update |

Use `notMerge` or `replaceMerge` when the number of series shrinks — plain merge keeps the old extra series.

For very large, streaming data, [`appendData`](https://echarts.apache.org/api.html#echartsInstance.appendData) adds points without a full re-render (not supported with `dataset`).
