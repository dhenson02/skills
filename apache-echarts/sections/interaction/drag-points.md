# Draggable Points (graphic component)

Pattern for custom interactivity: overlay invisible, draggable [`graphic`](https://echarts.apache.org/option.html#graphic) elements on series points, convert pixel ↔ data coordinates, and push updates via `setOption`.

Key APIs:

- [`convertToPixel('grid', [x, y])`](https://echarts.apache.org/api.html#echartsInstance.convertToPixel) — data → canvas px (origin top-left). `'grid'` = first grid; also `{ gridIndex }`, `{ geoIndex }`, `{ seriesIndex }`.
- [`convertFromPixel('grid', [px, py])`](https://echarts.apache.org/api.html#echartsInstance.convertFromPixel) — inverse.
- Both work only **after the first `setOption`** (coordinate systems must exist).

## Full example

```js
import * as echarts from 'echarts';

const symbolSize = 20;
const data = [[15, 0], [-50, 10], [-56.5, 20], [-46.5, 30], [-22.1, 40]];
const chart = echarts.init(document.getElementById('main'));

chart.setOption({
  tooltip: {
    triggerOn: 'none',   // disable default show/hide; driven manually below
    formatter: (params) => `X: ${params.data[0].toFixed(2)}<br>Y: ${params.data[1].toFixed(2)}`
  },
  xAxis: { min: -100, max: 80, type: 'value', axisLine: { onZero: false } },
  yAxis: { min: -30, max: 60, type: 'value', axisLine: { onZero: false } },
  series: [{ id: 'a', type: 'line', smooth: true, symbolSize, data }]  // big symbols ease dragging
});

// Overlay one invisible draggable circle per point (after first setOption)
chart.setOption({
  graphic: data.map((item, dataIndex) => ({
    type: 'circle',
    position: chart.convertToPixel('grid', item),
    shape: { r: symbolSize / 2 },
    invisible: true,     // still receives mouse events
    draggable: true,
    z: 100,              // above series symbols
    ondrag: function () {
      onPointDragging(dataIndex, [this.x, this.y]);   // `this` = dragged element
    },
    onmousemove: () => showTooltip(dataIndex),
    onmouseout: () => hideTooltip()
  }))
});

function onPointDragging(dataIndex, pos) {
  data[dataIndex] = chart.convertFromPixel('grid', pos);
  chart.setOption({ series: [{ id: 'a', data }] });
}

function showTooltip(dataIndex) {
  chart.dispatchAction({ type: 'showTip', seriesIndex: 0, dataIndex });
}

function hideTooltip() {
  chart.dispatchAction({ type: 'hideTip' });
}

// Graphic elements use pixel positions — recompute on resize
window.addEventListener('resize', () => {
  chart.resize();
  chart.setOption({
    graphic: data.map((item) => ({ position: chart.convertToPixel('grid', item) }))
  });
});
```

Extensions: add `dataZoom` (recompute graphic positions on `datazoom` too), or build a point editor on any coordinate system.
