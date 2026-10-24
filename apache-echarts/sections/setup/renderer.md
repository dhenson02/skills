# Canvas vs SVG Renderer

Canvas is the default. SVG is available since v4 and was rewritten with a virtual DOM in v5.3 (2–10× faster).

## Choosing

| Situation | Renderer |
| --- | --- |
| Large data (> ~1k elements), heatmaps, large line/scatter, effects | Canvas |
| Many chart instances on one page, low-end mobile, memory-constrained | SVG |
| Crisp output when zoomed, small memory footprint | SVG |
| Server-side string rendering | SVG (see [ssr/server-rendering.md](../ssr/server-rendering.md)) |
| Trail effect (`series-lines.effect`), heatmap blending | Canvas only |

With a good environment and modest data, either works — don't agonize. When optimizing, measure both.

## Usage

Full import (`import * as echarts from 'echarts'`) registers both renderers. Tree-shaken imports must register what they use:

```js
import * as echarts from 'echarts/core';
import { SVGRenderer, CanvasRenderer } from 'echarts/renderers';
echarts.use([SVGRenderer, CanvasRenderer]);
```

Select per instance:

```js
echarts.init(dom);                              // canvas (default)
echarts.init(dom, null, { renderer: 'canvas' });
echarts.init(dom, null, { renderer: 'svg' });
```
