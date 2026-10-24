# Data Transform

Declarative `outData = f(inData)` on datasets (since v5). Built-ins: `filter`, `sort` — syntax in [transform-filter-sort.md](transform-filter-sort.md). Others (regression, clustering, aggregate, histogram, boxplot) via external transforms.

Tree-shaken builds must register both `DatasetComponent` and `TransformComponent`.

## Basics

A dataset with `transform` derives its data from another dataset: `fromDatasetIndex` / `fromDatasetId`, defaulting to `fromDatasetIndex: 0`. Series reference the derived dataset via `datasetIndex`.

```js
option = {
  dataset: [
    {
      // datasetIndex 0: raw data
      source: [
        ['Product', 'Sales', 'Price', 'Year'],
        ['Cake', 123, 32, 2011],
        ['Cereal', 231, 14, 2011],
        ['Tofu', 235, 5, 2011],
        ['Cake', 143, 30, 2012],
        ['Cereal', 201, 19, 2012],
        ['Tofu', 255, 7, 2012],
        ['Cake', 153, 28, 2013],
        ['Cereal', 181, 21, 2013],
        ['Tofu', 395, 4, 2013]
      ]
    },
    // datasetIndex 1..3: filtered views of dataset 0
    { transform: { type: 'filter', config: { dimension: 'Year', value: 2011 } } },
    { transform: { type: 'filter', config: { dimension: 'Year', value: 2012 } } },
    { transform: { type: 'filter', config: { dimension: 'Year', value: 2013 } } }
  ],
  series: [
    { type: 'pie', radius: 50, center: ['25%', '50%'], datasetIndex: 1 },
    { type: 'pie', radius: 50, center: ['50%', '50%'], datasetIndex: 2 },
    { type: 'pie', radius: 50, center: ['75%', '50%'], datasetIndex: 3 }
  ]
};
```

## Piped transforms

An array runs transforms in sequence, each taking the previous output.

```js
{
  transform: [
    { type: 'filter', config: { dimension: 'Product', value: 'Tofu' } },
    { type: 'sort', config: { dimension: 'Year', order: 'desc' } }
  ]
}
```

Inside a pipe, only the first transform may take multiple inputs and only the last may produce multiple outputs.

## Multiple outputs

Some transforms emit several results (e.g. `boxplot`: result[0] = boxes, result[1] = outliers). Referencing the dataset yields result[0]; expose others with [`fromTransformResult`](https://echarts.apache.org/option.html#dataset.fromTransformResult):

```js
option = {
  dataset: [
    { source: [/* ... */] },
    { transform: { type: 'boxplot' } },                 // index 1 → result[0]
    { fromDatasetIndex: 1, fromTransformResult: 1 }     // index 2 → result[1]
  ],
  xAxis: { type: 'category' },
  yAxis: {},
  series: [
    { name: 'boxplot', type: 'boxplot', datasetIndex: 1 },
    { name: 'outlier', type: 'scatter', datasetIndex: 2 }
  ]
};
```

`fromTransformResult` and `transform` can coexist — the transform then consumes the selected upstream result:

```js
{
  fromDatasetIndex: 1,
  fromTransformResult: 1,
  transform: { type: 'sort', config: { dimension: 2, order: 'desc' } }
}
```

## Debugging

`print: true` logs a transform's output via `console.log` (dev builds only):

```js
{ transform: { type: 'filter', config: { /* ... */ }, print: true } }
```

## External transforms

Register first; external types are namespaced (`'ecStat:regression'`), built-ins are not. Example with [echarts-stat](https://github.com/ecomfe/echarts-stat):

```js
echarts.registerTransform(ecStatTransform(ecStat).regression);

option = {
  dataset: [
    { source: rawData },
    { transform: { type: 'ecStat:regression', config: { method: 'exponential' } } }
  ],
  xAxis: { type: 'category' },
  yAxis: {},
  series: [
    { name: 'scatter', type: 'scatter', datasetIndex: 0 },
    { name: 'regression', type: 'line', symbol: 'none', datasetIndex: 1 }
  ]
};
```

echarts-stat examples: [aggregate](https://echarts.apache.org/examples/en/editor.html?c=data-transform-aggregate), [histogram](https://echarts.apache.org/examples/en/editor.html?c=bar-histogram), [clustering](https://echarts.apache.org/examples/en/editor.html?c=scatter-clustering), regression: [linear](https://echarts.apache.org/examples/en/editor.html?c=scatter-linear-regression), [exponential](https://echarts.apache.org/examples/en/editor.html?c=scatter-exponential-regression), [logarithmic](https://echarts.apache.org/examples/en/editor.html?c=scatter-logarithmic-regression), [polynomial](https://echarts.apache.org/examples/en/editor.html?c=scatter-polynomial-regression).
