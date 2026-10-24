# Events and Actions

- **Events** — listen with `chart.on(name, [query], handler)`; remove with `chart.off(name, handler)`. Full list: [API events](https://echarts.apache.org/api.html#events).
- **Actions** — trigger behavior programmatically with `chart.dispatchAction({ type, ... })`. Full list: [API actions](https://echarts.apache.org/api.html#action).

## Contents
- Mouse events and params
- Query filters
- Component interaction events
- dispatchAction
- Blank-area clicks (zrender)
- Pointer snapping

## Mouse events

`'click'`, `'dblclick'`, `'mousedown'`, `'mousemove'`, `'mouseup'`, `'mouseover'`, `'mouseout'`, `'globalout'`, `'contextmenu'`. Fire only on graphic elements.

```js
chart.on('click', (params) => {
  window.open('https://www.google.com/search?q=' + encodeURIComponent(params.name));
});
```

`params`:

```ts
type EventParams = {
  componentType: string;   // 'series' | 'markLine' | 'markPoint' | 'timeline' | ...
  seriesType: string;      // 'line' | 'bar' | 'pie' | ... (when componentType === 'series')
  seriesIndex: number;
  seriesName: string;
  name: string;            // data (category) name
  dataIndex: number;
  data: Object;            // raw data item
  dataType: string;        // 'node' | 'edge' for graph/sankey; otherwise unused
  value: number | Array;
  color: string;
};
```

```js
chart.on('click', (params) => {
  if (params.componentType === 'markPoint' && params.seriesIndex === 5) {
    // markPoint of series 5
  } else if (params.componentType === 'series' && params.seriesType === 'graph') {
    // params.dataType === 'edge' ? edge : node
  }
});
```

Drill-down pattern — fetch detail on click and update another series:

```js
chart.on('click', async (params) => {
  const detail = await fetch('detail?q=' + encodeURIComponent(params.name)).then((r) => r.json());
  chart.setOption({ series: [{ name: 'pie', data: detail.data }] });
});
```

## Query filters

`chart.on(eventName, query, handler)` fires only for matching targets.

String: `'mainType'` or `'mainType.subType'`:

```js
chart.on('click', 'series', handler);
chart.on('click', 'series.line', handler);
chart.on('click', 'dataZoom', handler);
chart.on('click', 'xAxis.category', handler);
```

Object keys: `${mainType}Index`, `${mainType}Name`, `${mainType}Id`, `dataIndex`, `name`, `dataType`, `element` (custom series element name).

```js
chart.on('mouseover', { seriesName: 'uuu' }, handler);
chart.on('mouseover', { seriesIndex: 1, name: 'xx' }, handler);   // data 'xx' in series 1
chart.on('click', { dataType: 'node' }, handler);                  // graph nodes
chart.on('click', { dataType: 'edge' }, handler);                  // graph edges
chart.on('mouseup', { element: 'my_el' }, handler);                // custom series: renderItem child with name 'my_el'
```

## Component interaction events

Every component interaction emits an event, e.g. `legendselectchanged`, `datazoom`, `geoselectchanged`, `brushselected`.

```js
chart.on('legendselectchanged', (params) => {
  const isSelected = params.selected[params.name];
  console.log((isSelected ? 'Selected ' : 'Unselected ') + params.name);
  console.log(params.selected);   // all legend states
});
```

## dispatchAction

Common types: `highlight`, `downplay`, `select`, `unselect`, `showTip`, `hideTip`, `legendSelect`, `legendUnSelect`, `legendToggleSelect`, `dataZoom`.

Cycle highlight + tooltip through pie slices:

```js
option = {
  tooltip: { trigger: 'item', formatter: '{a} <br/>{b} : {c} ({d}%)' },
  legend: { orient: 'vertical', left: 'left' },
  series: [{
    name: 'Access Source',
    type: 'pie',
    radius: '55%',
    center: ['50%', '60%'],
    data: [
      { value: 335, name: 'Direct Access' },
      { value: 310, name: 'Email Marketing' },
      { value: 234, name: 'Affiliate Ads' },
      { value: 135, name: 'Video Ads' },
      { value: 1548, name: 'Search Engines' }
    ],
    emphasis: { itemStyle: { shadowBlur: 10, shadowOffsetX: 0, shadowColor: 'rgba(0, 0, 0, 0.5)' } }
  }]
};

let currentIndex = -1;
setInterval(() => {
  const dataLen = option.series[0].data.length;
  chart.dispatchAction({ type: 'downplay', seriesIndex: 0, dataIndex: currentIndex });
  currentIndex = (currentIndex + 1) % dataLen;
  chart.dispatchAction({ type: 'highlight', seriesIndex: 0, dataIndex: currentIndex });
  chart.dispatchAction({ type: 'showTip', seriesIndex: 0, dataIndex: currentIndex });
}, 1000);
```

## Blank-area clicks

ECharts events fire only on elements. Listen on the underlying zrender instance for clicks anywhere; no `target` means blank space.

```js
chart.getZr().on('click', (event) => {
  if (!event.target) {
    // clicked blank area — e.g. reset drill-down
  }
});
```

## Pointer snapping

Since v5.4, on mobile, a tap near (not on) a small element snaps to the nearest element within a radius (default 44px, per WCAG target size). Disabled for `large: true` series.

```js
echarts.init(dom, null, {
  useCoarsePointer: true,   // true: all platforms; false: never; default: mobile only
  pointerSize: 44           // snap radius (px)
});
```
