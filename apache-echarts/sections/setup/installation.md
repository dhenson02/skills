# Installation and Imports

## CDN (no build step)

```html
<script src="https://cdn.jsdelivr.net/npm/echarts/dist/echarts.min.js"></script>
<!-- exposes global `echarts` -->
```

Pin a major version in production, e.g. `npm/echarts@6/dist/echarts.min.js`.

## npm — full import

```bash
npm install echarts
```

```js
import * as echarts from 'echarts';

const chart = echarts.init(document.getElementById('main'));
chart.setOption({
  xAxis: { data: ['shirt', 'cardigan', 'chiffon', 'pants', 'heels', 'socks'] },
  yAxis: {},
  series: [{ name: 'sales', type: 'bar', data: [5, 20, 36, 10, 10, 20] }]
});
```

Registers every chart, component, and both renderers. Simplest, largest bundle.

## npm — tree-shaken import (smaller bundle)

Register only what the option uses. Anything missing silently fails to render (or logs a console error), so match the list to the option.

```js
import * as echarts from 'echarts/core';
import { BarChart, LineChart } from 'echarts/charts';          // suffix: Chart
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,          // xAxis/yAxis cartesian grid
  LegendComponent,
  DatasetComponent,
  TransformComponent      // required for dataset.transform (filter/sort)
} from 'echarts/components';                                   // suffix: Component
import { LabelLayout, UniversalTransition } from 'echarts/features';
import { CanvasRenderer } from 'echarts/renderers';            // or SVGRenderer — one is REQUIRED

echarts.use([
  BarChart, LineChart,
  TitleComponent, TooltipComponent, GridComponent, LegendComponent,
  DatasetComponent, TransformComponent,
  LabelLayout, UniversalTransition,
  CanvasRenderer
]);
```

The tree-shakeable entry ships no renderer by default; importing only `SVGRenderer` keeps the canvas renderer out of the bundle.

The "Full Code" tab in the [online example editor](https://echarts.apache.org/examples) generates the exact import list for any option.

Components that are commonly forgotten: `AriaComponent` (accessibility), `DataZoomComponent`, `VisualMapComponent`, `ToolboxComponent`, `MarkLineComponent`/`MarkPointComponent`/`MarkAreaComponent`, `GraphicComponent`, `PolarComponent`, `GeoComponent`.

## TypeScript: strict option type

`ComposeOption` builds an option type restricted to the registered parts, catching options that need an unregistered component.

```ts
import * as echarts from 'echarts/core';
import { BarChart, LineChart } from 'echarts/charts';
import {
  TitleComponent, TooltipComponent, GridComponent,
  DatasetComponent, TransformComponent
} from 'echarts/components';
import { LabelLayout, UniversalTransition } from 'echarts/features';
import { CanvasRenderer } from 'echarts/renderers';
import type { BarSeriesOption, LineSeriesOption } from 'echarts/charts';   // suffix: SeriesOption
import type {
  TitleComponentOption, TooltipComponentOption,
  GridComponentOption, DatasetComponentOption
} from 'echarts/components';                                             // suffix: ComponentOption
import type { ComposeOption } from 'echarts/core';

type ECOption = ComposeOption<
  | BarSeriesOption
  | LineSeriesOption
  | TitleComponentOption
  | TooltipComponentOption
  | GridComponentOption
  | DatasetComponentOption
>;

echarts.use([
  TitleComponent, TooltipComponent, GridComponent, DatasetComponent, TransformComponent,
  BarChart, LineChart, LabelLayout, UniversalTransition, CanvasRenderer
]);

const option: ECOption = { /* ... */ };
```

With the full import, use `echarts.EChartsOption` as the option type.
