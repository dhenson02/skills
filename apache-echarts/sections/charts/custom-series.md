# Custom Series

`series.type: 'custom'` renders arbitrary graphics per data item via `renderItem`. Options: [series-custom](https://echarts.apache.org/option.html#series-custom).

## Registerable custom series (v6+)

Prebuilt custom series ship as npm packages in [echarts-custom-series](https://github.com/apache/echarts-custom-series) (e.g. range bar). Each README lists its `itemPayload` params and recommended `encode`.

```bash
npm install @echarts-x/custom-bar-range
```

Without a bundler, the `*.auto.js` build self-registers on the global `echarts`:

```html
<script src="./node_modules/echarts/dist/echarts.js"></script>
<script src="./node_modules/@echarts-x/custom-bar-range/dist/bar-range.auto.js"></script>
<script>
  const chart = echarts.init(document.getElementById('main'));
  chart.setOption({
    xAxis: { type: 'category' },
    yAxis: {},
    series: [{
      type: 'custom',
      renderItem: 'barRange',                 // registered name, not a function
      data: [
        [0, 26.7, 32.5],
        [1, 25.3, 32.4],
        [2, 24.6, 32.7],
        [3, 26.8, 35.8]
      ],
      itemPayload: { barWidth: 10, borderRadius: 5 },   // series-specific params
      encode: { x: 0, y: [1, 2], tooltip: [1, 2] }
    }]
  });
</script>
```

With a bundler, follow the package README for the import/registration call.

### Writing a reusable one

Fork echarts-custom-series and run `npm run generate <name>` for scaffolding (build, docs, examples). The core work is implementing [`renderItem`](https://echarts.apache.org/option.html#series-custom.renderItem).

## Inline renderItem (one-off)

Pass a function returning graphic elements. `api.value(dim)` reads data; `api.coord([x, y])` converts to pixels; `api.size([dx, dy])` converts data extents to pixel sizes.

```js
series: [{
  type: 'custom',
  renderItem: (params, api) => {
    const start = api.coord([api.value(0), api.value(1)]);
    const size = api.size([0, api.value(2) - api.value(1)]);
    return {
      type: 'rect',
      shape: { x: start[0] - 5, y: start[1] - size[1], width: 10, height: size[1] },
      style: api.style()
    };
  },
  data: [[0, 26.7, 32.5], [1, 25.3, 32.4]],
  encode: { x: 0, y: [1, 2] }
}]
```

Name elements (`name: 'my_el'`) to target them in events: `chart.on('click', { element: 'my_el' }, fn)`.

More: [official custom series examples](https://echarts.apache.org/examples#chart-type-custom).
