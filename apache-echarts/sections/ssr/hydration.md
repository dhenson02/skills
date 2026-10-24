# SSR + Client Interactivity

Server rendering: [server-rendering.md](server-rendering.md).

## Option A: lazy-load full ECharts

1. Serve the SSR SVG for instant display.
2. Load `echarts.js` in the background.
3. Re-render the same option client-side with `echarts.init` on the container, with interactive components enabled (`tooltip: { show: true }`) and `animation: false` (the SSR SVG already played it).

Full interactivity; nothing responds until the script loads (show a progress indicator, e.g. [pace-js](https://www.npmjs.com/package/pace-js)). The standard choice.

## Option B: lightweight client runtime (v5.5+, < 4 KB)

Supports, on top of SSR SVG:

- Initial animation and hover highlight (CSS in the SVG).
- Legend toggling and data changes — by requesting a **new SVG from the server**.

Not supported: real-time interactions like tooltips on mouse move. Requires the client or server to track state (HTTP is stateless; prefer client-side state over server sessions). Use when bundle size is critical.

```html
<script src="https://cdn.jsdelivr.net/npm/echarts/ssr/client/dist/index.min.js"></script>
<!-- or: node_modules/echarts/ssr/client/dist/index.js -->
```

```html
<div id="chart-container" style="width:800px;height:600px"></div>
<script>
  const ssrClient = window['echarts-ssr-client'];
  const isSeriesShown = { a: true, b: true };

  function updateChart(svgStr) {
    const container = document.getElementById('chart-container');
    container.innerHTML = svgStr;   // trusted server output only

    ssrClient.hydrate(container, {
      on: {
        click: (params) => {
          if (params.ssrType === 'legend') {
            isSeriesShown[params.seriesName] = !isSeriesShown[params.seriesName];
            fetch('/chart?series=' + encodeURIComponent(JSON.stringify(isSeriesShown)))
              .then((res) => res.text())
              .then(updateChart);
          }
        }
      }
    });
  }

  fetch('/chart').then((res) => res.text()).then(updateChart);
</script>
```

The server re-renders using the SVG method with the received state.

### API

`hydrate(dom: HTMLElement, options: ECSSRClientOptions)` — `dom` must already contain the server SVG.

```ts
type ECSSRClientOptions = {
  on?: {
    mouseover?: (params: ECSSRClientEventParams) => void;
    mouseout?: (params: ECSSRClientEventParams) => void;
    click?: (params: ECSSRClientEventParams) => void;
  };
};

type ECSSRClientEventParams = {
  type: 'mouseover' | 'mouseout' | 'click';
  ssrType: 'legend' | 'chart';   // legend item vs data item
  seriesIndex?: number;
  dataIndex?: number;
  event: Event;                  // native event
};
```

Events target chart items (bars, points), not the container.

## Choosing

| Solution | Load size | Lost features | Dev effort | Use when |
| --- | --- | --- | --- | --- |
| Client, full import | Largest | None | Minimal | First paint not critical |
| Client, tree-shaken | Large | Unimported features | Small | Want smaller bundle, no server |
| SSR SVG only | Small | Dynamic data, legend toggle, tooltips | Medium | Fast first paint, static chart |
| SSR Canvas only | Large (image) | Above + initial animation; blurry zoom | Medium | SVG not accepted |
| SSR SVG + lazy full ECharts | Small → large | Nothing after load | Medium | Fast first paint + full interaction |
| SSR SVG + light runtime | Small | Real-time interactions | Large (state + protocol) | Strict size budget |
| SSR SVG + light runtime, then lazy full | Small → large | Complex interaction before load | Largest | Both, with time to build |
