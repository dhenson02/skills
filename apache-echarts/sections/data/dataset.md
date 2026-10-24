# Dataset

[`dataset`](https://echarts.apache.org/option.html#dataset) holds data separately from series config (since v4). Prefer it over `series.data` for tabular data: one source feeds many series, data swaps independently of styling, and common formats (2D array, array of objects) work without reshaping.

Keep `series.data` for non-tabular series (`tree`, `treemap`, `graph`, `sankey`, `lines`) and for million-point streaming via [`appendData`](https://echarts.apache.org/api.html#echartsInstance.appendData), which `dataset` does not support. If a series declares `series.data`, it ignores `dataset`.

Series supporting `dataset`: `line`, `bar`, `pie`, `scatter`, `effectScatter`, `parallel`, `candlestick`, `map`, `funnel`, `custom`.

Mapping details (dimensions, `encode`, rows vs columns): [dataset-encode.md](dataset-encode.md). Filtering/sorting: [transform.md](transform.md).

## series.data vs dataset

```js
// series.data: data split per series by hand
option = {
  xAxis: { type: 'category', data: ['Matcha Latte', 'Milk Tea', 'Cheese Cocoa', 'Walnut Brownie'] },
  yAxis: {},
  series: [
    { type: 'bar', name: '2015', data: [89.3, 92.1, 94.4, 85.4] },
    { type: 'bar', name: '2016', data: [95.8, 89.4, 91.2, 76.9] },
    { type: 'bar', name: '2017', data: [97.7, 83.1, 92.5, 78.1] }
  ]
};
```

```js
// dataset: one table, series map to columns by default
option = {
  legend: {},
  tooltip: {},
  dataset: {
    source: [
      ['product', '2015', '2016', '2017'],   // header row → dimension names
      ['Matcha Latte', 43.3, 85.8, 93.7],
      ['Milk Tea', 83.1, 73.4, 55.1],
      ['Cheese Cocoa', 86.4, 65.2, 82.5],
      ['Walnut Brownie', 72.4, 53.9, 39.1]
    ]
  },
  xAxis: { type: 'category' },  // category axis takes the first column
  yAxis: {},
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]  // one per remaining column
};
```

## Source formats

```js
// 2D array (supports seriesLayoutBy)
dataset: { source: [['product', 'count'], ['Latte', 823], ['Tea', 235]] }

// Array of objects — declare dimensions to fix column order
dataset: {
  dimensions: ['product', '2015', '2016', '2017'],
  source: [
    { product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7 },
    { product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1 }
  ]
}

// Object of columns
dataset: {
  source: {
    product: ['Matcha Latte', 'Milk Tea', 'Cheese Cocoa'],
    count: [823, 235, 1042],
    score: [95.8, 81.4, 91.2]
  }
}
```

`seriesLayoutBy` is not supported for key-value formats. For CSV input, parse to arrays first (e.g. [d3-dsv](https://github.com/d3/d3-dsv), [PapaParse](https://github.com/mholt/PapaParse)).

## Multiple datasets

`dataset` can be an array; series choose one with [`datasetIndex`](https://echarts.apache.org/option.html#series.datasetIndex) (default `0`).

```js
option = {
  dataset: [{ source: [/* ... */] }, { source: [/* ... */] }],
  series: [
    { type: 'bar', datasetIndex: 1 },
    { type: 'line', datasetIndex: 0 }
  ]
};
```

Example of several charts sharing one dataset with linked interaction: [dataset-link](https://echarts.apache.org/examples/en/editor.html?c=dataset-link).
