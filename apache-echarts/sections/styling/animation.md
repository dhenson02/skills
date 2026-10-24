# Animation and Transitions

`setOption` diffs new data against old **by data `name`** (then index) and animates three states: enter (added), update (changed), leave (removed). No manual animation code needed.

```js
function randomData() {
  return ['A', 'B', 'C'].map((name) => ({ name, value: Math.random() }));
}

option = { series: [{ type: 'pie', radius: [0, '50%'], data: randomData() }] };

setInterval(() => {
  chart.setOption({ series: { data: randomData() } });
}, 2000);
```

Old names `A, B, C` → new `B, C, D`: B, C update; A leaves; D enters.

## Options

Set at option root (all series/components) or per series.

| Enter | Update | Meaning |
| --- | --- | --- |
| `animationDuration` | `animationDurationUpdate` | ms; `0` disables that phase |
| `animationEasing` | `animationEasingUpdate` | Easing name or `(t) => progress` |
| `animationDelay` | `animationDelayUpdate` | ms or `(idx) => ms` for staggering |

- `animation: false` disables all animation.
- Built-in easings include `'linear'`, `'quadraticIn/Out/InOut'`, `'cubicIn/Out/InOut'`, `'elasticOut'`, `'bounceOut'`, `'backOut'`, and more.

Staggered bars:

```js
const xAxisData = [];
const data1 = [];
const data2 = [];
for (let i = 0; i < 100; i++) {
  xAxisData.push('A' + i);
  data1.push((Math.sin(i / 5) * (i / 5 - 10) + i / 6) * 5);
  data2.push((Math.cos(i / 5) * (i / 5 - 10) + i / 6) * 5);
}

option = {
  legend: { data: ['bar', 'bar2'] },
  xAxis: { data: xAxisData, splitLine: { show: false } },
  yAxis: {},
  series: [
    { name: 'bar', type: 'bar', data: data1, emphasis: { focus: 'series' }, animationDelay: (idx) => idx * 10 },
    { name: 'bar2', type: 'bar', data: data2, emphasis: { focus: 'series' }, animationDelay: (idx) => idx * 10 + 100 }
  ],
  animationEasing: 'elasticOut',
  animationDelayUpdate: (idx) => idx * 5
};
```

## Performance

- Large data: `animation: false`.
- Changing data volume: `animationThreshold` (default `2000`) auto-disables animation above that many graphic elements. Lower it for complex charts or busy pages.

## Waiting for render to finish

Without animation, rendering is synchronous after `setOption`:

```js
chart.setOption({ animation: false /* ... */ });
const dataUrl = chart.getDataURL();   // final image
```

With animation, `getDataURL()` right away captures the first frame. Wait for the `finished` event (or `rendered` for every frame):

```js
function onFinished() {
  const dataUrl = chart.getDataURL();
  chart.off('finished', onFinished);   // also fires after later interactions
}
chart.on('finished', onFinished);
chart.setOption({ animationDuration: 1000 /* ... */ });
```
