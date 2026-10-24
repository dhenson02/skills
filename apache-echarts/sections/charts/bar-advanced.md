# Bar Race and Waterfall

Basics: [bar.md](bar.md).

## Bar race (dynamic sorting, v5+)

Horizontal bars re-ranking over time. For vertical bars, swap the x/y settings below.

| Setting | Value | Why |
| --- | --- | --- |
| `series.realtimeSort` | `true` | Enables re-ordering |
| `yAxis.inverse` | `true` | Largest bar on top |
| `yAxis.animationDuration` / `animationDurationUpdate` | `300` | Re-order animation speed |
| `yAxis.max` | `n - 1` | Show only top *n* bars |
| `xAxis.max` | `'dataMax'` | Scale to current maximum |
| `series.label.valueAnimation` | `true` | Animate label numbers |
| `animationDuration` | `0` | First frame doesn't grow from 0 |
| `animationDurationUpdate` | `3000` | Must equal the `setOption` interval |
| `animationEasing` / `animationEasingUpdate` | `'linear'` | Constant-speed motion |

```js
const data = Array.from({ length: 5 }, () => Math.round(Math.random() * 200));

option = {
  xAxis: { max: 'dataMax' },
  yAxis: {
    type: 'category',
    data: ['A', 'B', 'C', 'D', 'E'],
    inverse: true,
    animationDuration: 300,
    animationDurationUpdate: 300,
    max: 2                       // top 3 only
  },
  series: [{
    realtimeSort: true,
    name: 'X',
    type: 'bar',
    data,
    label: { show: true, position: 'right', valueAnimation: true }
  }],
  legend: { show: true },
  animationDuration: 0,
  animationDurationUpdate: 3000,
  animationEasing: 'linear',
  animationEasingUpdate: 'linear'
};

function run() {
  const values = option.series[0].data;
  for (let i = 0; i < values.length; ++i) {
    values[i] += Math.round(Math.random() * (Math.random() > 0.9 ? 2000 : 200));
  }
  chart.setOption(option);
}

setTimeout(run, 0);
setInterval(run, 3000);
```

## Waterfall

No native series; simulate with three stacked bar series: a transparent "helper" that lifts bars, one for increases, one for decreases. `'-'` marks an empty slot.

```js
const deltas = [900, 345, 393, -108, -154, 135, 178, 286, -119, -361, -203];
const help = [];
const positive = [];
const negative = [];

for (let i = 0, sum = 0; i < deltas.length; ++i) {
  if (deltas[i] >= 0) {
    positive.push(deltas[i]);
    negative.push('-');
  } else {
    positive.push('-');
    negative.push(-deltas[i]);
  }

  if (i === 0) {
    help.push(0);
  } else {
    sum += deltas[i - 1];
    help.push(deltas[i] < 0 ? sum + deltas[i] : sum);
  }
}

option = {
  title: { text: 'Waterfall' },
  grid: { left: '3%', right: '4%', bottom: '3%', containLabel: true },
  xAxis: {
    type: 'category',
    splitLine: { show: false },
    data: deltas.map((_, i) => 'Oct/' + (i + 1))
  },
  yAxis: { type: 'value' },
  series: [
    {
      type: 'bar',
      stack: 'all',
      silent: true,                                             // non-interactive
      itemStyle: { borderColor: 'transparent', color: 'transparent' },
      emphasis: { itemStyle: { borderColor: 'transparent', color: 'transparent' } },
      data: help
    },
    { name: 'positive', type: 'bar', stack: 'all', data: positive },
    { name: 'negative', type: 'bar', stack: 'all', data: negative, itemStyle: { color: '#f33' } }
  ]
};
```
