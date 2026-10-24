# Accessibility (ARIA and Decals)

Off by default. Enable with [`aria.enabled`](https://echarts.apache.org/option.html#aria.enabled) (`aria.show` in older docs).

Tree-shaken builds must register the component, or `aria` silently does nothing:

```js
import { AriaComponent } from 'echarts/components';
echarts.use(AriaComponent);
```

## Generated description

ECharts adds an `aria-label` to the chart container, built from title, series, and data.

```js
option = {
  aria: { enabled: true },
  title: { text: 'Referrer of a User', left: 'center' },
  series: [{
    name: 'Referrer',
    type: 'pie',
    data: [
      { value: 335, name: 'Direct Visit' },
      { value: 310, name: 'Email Marketing' },
      { value: 234, name: 'Union Ad' },
      { value: 135, name: 'Video Ad' },
      { value: 1548, name: 'Search Engine' }
    ]
  }]
};
```

Produces roughly: *This is a chart about "Referrer of a User" with type Pie chart named Referrer. The data is as follows: the data of Direct Visit is 335, …* — in the configured `locale`.

## Customizing

- **Title part:** [`aria.general.withTitle`](https://echarts.apache.org/option.html#aria.general.withTitle) (`{title}` placeholder) / [`aria.general.withoutTitle`](https://echarts.apache.org/option.html#aria.general.withoutTitle).
- **Whole description:** [`aria.description`](https://echarts.apache.org/option.html#aria.description) — use for large datasets where listing every item is useless (e.g. dense scatter).
- **Templates for every part:** [`aria.label`](https://echarts.apache.org/option.html#aria.label).

```js
aria: {
  enabled: true,
  description: 'Scatter of 2,000 households: income correlates positively with spending.'
}
```

## Decal patterns

Textures as a second channel beside color, for color-blind users (v5+):

```js
aria: {
  enabled: true,
  decal: { show: true }            // default patterns
  // decal: { show: true, decals: [{ symbol: 'rect', dashArrayX: [1, 0], dashArrayY: [2, 5] }] }
}
```

Custom patterns: [`aria.decal.decals`](https://echarts.apache.org/option.html#aria.decal.decals).
