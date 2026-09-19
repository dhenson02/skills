# Bar Chart

*Source: [Basic Bar - Bar - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/bar/basic-bar)*

## Basic Bar Chart

Bar Chart, is a chart that presents the comparisons among discrete data. The length of the bars is proportionally related to the categorical data.

To set the bar chart, you need to set the `type` of `series` as `'bar'`.

[\[Option\]](https://echarts.apache.org/option.html#series-bar)

### Simple Example

Let's begin with a basic bar chart:

```
option = {
  xAxis: {
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [23, 24, 18, 25, 27, 28, 25]
    }
  ]
};
```

live

In this case, the x-axis is under the category type. Therefore, you should define some corresponding values for `'xAxis'`. Meanwhile, the data type of the y-axis is numerical. The range of the y-axis will be generated automatically by the `data` in `'series'`.

### Multi-series Bar Chart

You may use a series to represent a group of related data. To show multiple series in the same chart, you need to add one more array under the `series`.

```
option = {
  xAxis: {
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [23, 24, 18, 25, 27, 28, 25]
    },
    {
      type: 'bar',
      data: [26, 24, 18, 22, 23, 20, 27]
    }
  ]
};
```

live

### Customized Bar Chart

#### Styles

It is a good idea to install the style of Bar Chart by using ['series.itemStyle'](https://echarts.apache.org/option.html#series-bar.itemStyle). Description of the SCI:

- Color of column(`'color'`);
- Outline color(`'borderColor'`), width(`'borderWidth'`), type(`'borderType'`) of column;
- Border radius of column(`'barBorderRadius'`);
- Transparency(`'opacity'`);
- Shadow type(`'shadowBlur'`, `'shadowColor'`, `'shadowOffsetX'`, `'shadowOffsetY'`)

Here is a example:

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [
        10,
        22,
        28,
        {
          value: 43,
          // Specify the style for single bar
          itemStyle: {
            color: '#91cc75',
            shadowColor: '#91cc75',
            borderType: 'dashed',
            opacity: 0.5
          }
        },
        49
      ],
      itemStyle: {
        barBorderRadius: 5,
        borderWidth: 1,
        borderType: 'solid',
        borderColor: '#73c0de',
        shadowColor: '#5470c6',
        shadowBlur: 3
      }
    }
  ]
};
```

live

In this case, we defined the style of the bar chart by `'itemStyle'` of corresponding `series`. For complete configuration items and their usage, please check the configuration item manual: [`series.itemStyle`](https://echarts.apache.org/option.html#series-bar.itemStyle)。

#### Width and Height of Column

You can use [`barWidth`](https://echarts.apache.org/option.html##series-bar.barWidth) to change the width of the bar. For instance, the `'barWidth'` in the following case was set to `'20%'`. It indicates that width of each bar is 20% of the category width. As there are 5 data in every series, 20% `'barWidth'` means 4% for the entire x-axis.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [10, 22, 28, 43, 49],
      barWidth: '20%'
    }
  ]
};
```

live

In addition, [`barMaxWidth`](https://echarts.apache.org/option.html#series-bar.barMaxWidth) has limited the maximum width of the bar. For some small value, you can declare a minimum height for the bar: [`barMinHeight`](https://echarts.apache.org/option.html#series-bar.barMinHeight). When the corresponding height of data is smaller than the limit, ECharts will take 'barMinHeight' as the replaced height.

#### Column Spacing

There are two kinds of column spacing. One is the spacing between different series under the same category: [`barGap`](https://echarts.apache.org/option.html#series-bar.barGap). The other is the spacing between categories: [`barCategoryGap`](https://echarts.apache.org/option.html#series-bar.barCategoryGap).

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      data: [23, 24, 18, 25, 18],
      barGap: '20%',
      barCategoryGap: '40%'
    },
    {
      type: 'bar',
      data: [12, 14, 9, 9, 11]
    }
  ]
};
```

live

In this case, the `barGap` is `'20%'`. That means the distance between bars under the same category is 20% of the bar width. For instance, if we set the `barCategoryGap` to `'40%'`, the gap on each side of the bar will take 40% place in categories (compared with the width of the column).

Usually, `barWidth` is not necessary to be clarified if `'barGap'` and `barCategoryGap` was set. If you need to make sure the bar is not too wide while the graph is large, try to use `barMaxWidth` to limit the maximum width of bars.

> In the same cartesian coordinate system, the property will be shared by several column series. To make sure it takes effect on the graph, please set the property on the last bar chart series of the system.

#### Add Background Color for Bars

You might want to change the background color of bars sometimes. After ECharts v4.7.0, this function can be enabled by ['showBackground'](https://echarts.apache.org/option.html#series-bar.showBackground) and configured by ['backgroundStyle'](https://echarts.apache.org/option.html#series-bar.backgroundStyle).

```
option = {
  xAxis: {
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      data: [120, 200, 150, 80, 70, 110, 130],
      type: 'bar',
      showBackground: true,
      backgroundStyle: {
        color: 'rgba(220, 220, 220, 0.8)'
      }
    }
  ]
};
```

live

*Source: [Stacked Bar - Bar - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/bar/stacked-bar)*

## Stacked Bar Chart

Sometimes, we hope to not only figure series separately but also the trend of the sum. It's a good choice to implement it by using the stacked bar chart. As the name suggests, in the stacked bar chart, data in the same category will be stacked up in one column. The overall height of the bar explained the change of total.

There is a simple way to implement a stacked bar chart by ECharts. You need to set the same string type value for a group of series in `stack`. The series with the same `stack` value will be in the same category.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 43, 49],
      type: 'bar',
      stack: 'x'
    },
    {
      data: [5, 4, 3, 5, 10],
      type: 'bar',
      stack: 'x'
    }
  ]
};
```

live

In this case, the position of the second series is based on the position of the first series, the height increased is corresponding to the first series' height. Therefore, from the position of the second series, you can find the changing trend of the sum of the two.

> The value of `stack` explained what series will be stacked up together. Theoretically, the specific value of 'stack' is meaningless. However, we prefer some suggestive strings for the convenience of reading.
> 
> For instance, here is a chart with 4 series that counted the amount of male and female. "adult male" and "boy" need to be stacked while "adult female" and "girl" need to be stacked. In this case, the suggestive value of `stack` is `'male'` and `'female'`. Although meaningless strings like `'a'` and `'b'` can achieve the same effect but the code will have worse readability.

*Source: [Bar Racing - Bar - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/bar/bar-race)*

## Dynamic Sorting Bar Chart

### Related Options

Bar race is a chart that shows changes in the ranking of data over time and it is supported by default since ECharts 5.

> Bar race charts usually use horizontal bars. If you want to use vertical bars, just take the X axis and Y axis in this tutorial to the opposite.

1. Set `realtimeSort` of the bar series to be `true` to enable bar race
2. Set `yAxis.inverse` to be `true` to display longer bars at top
3. `yAxis.animationDuration` is suggested to be set to be `300` for bar reordering animation for the first time
4. `yAxis.animationDurationUpdate` is suggested to be set to be `300` for bar reordering animation for later times
5. Set `yAxis.max` to be *n - 1* where *n* is the number of bars to be displayed; otherwise, all bars are displayed
6. `xAxis.max` is suggested to be set to be `'dataMax'` so that the maximum of data is used as X maximum.
7. If realtime label changing is required, set `series.label.valueAnimation` to be `true`
8. Set `animationDuration` to be `0` so that the first animation doesn't start from 0; if you wish otherwise, set it to be the same value as `animationDurationUpdate`
9. `animationDurationUpdate` is suggested to be set to be `3000` for animation update duration, which should be the same as the frequency of calling `setOption`
10. Call `setOption` to update data to be of next time with `setInterval` at the frequency of `animationDurationUpdate`

### Demo

A complete demo:

```
var data = [];
for (let i = 0; i < 5; ++i) {
  data.push(Math.round(Math.random() * 200));
}

option = {
  xAxis: {
    max: 'dataMax'
  },
  yAxis: {
    type: 'category',
    data: ['A', 'B', 'C', 'D', 'E'],
    inverse: true,
    animationDuration: 300,
    animationDurationUpdate: 300,
    max: 2 // only the largest 3 bars will be displayed
  },
  series: [
    {
      realtimeSort: true,
      name: 'X',
      type: 'bar',
      data: data,
      label: {
        show: true,
        position: 'right',
        valueAnimation: true
      }
    }
  ],
  legend: {
    show: true
  },
  animationDuration: 0,
  animationDurationUpdate: 3000,
  animationEasing: 'linear',
  animationEasingUpdate: 'linear'
};

function run() {
  var data = option.series[0].data;
  for (var i = 0; i < data.length; ++i) {
    if (Math.random() > 0.9) {
      data[i] += Math.round(Math.random() * 2000);
    } else {
      data[i] += Math.round(Math.random() * 200);
    }
  }
  myChart.setOption(option);
}

setTimeout(function() {
  run();
}, 0);
setInterval(function() {
  run();
}, 3000);
```

live

*Source: [Waterfall - Bar - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/bar/waterfall)*

## Waterfall

There is no waterfall series in Apache ECharts, but we can simulate the effect using a stacked bar chart.

Assuming that the values in the data array represent an increase or decrease from the previous value.

```js
var data = [900, 345, 393, -108, -154, 135, 178, 286, -119, -361, -203];
```

That is, the first data is `900` and the second data `345` represents the addition of `345` to `900`, etc. When presenting this data as a stepped waterfall chart, we can use three series: the first is a non-interactive transparent series to implement the suspension bar effect, the second series is used to represent positive numbers, and the third series is used to represent negative numbers.

```
var data = [900, 345, 393, -108, -154, 135, 178, 286, -119, -361, -203];
var help = [];
var positive = [];
var negative = [];
for (var i = 0, sum = 0; i < data.length; ++i) {
  if (data[i] >= 0) {
    positive.push(data[i]);
    negative.push('-');
  } else {
    positive.push('-');
    negative.push(-data[i]);
  }

  if (i === 0) {
    help.push(0);
  } else {
    sum += data[i - 1];
    if (data[i] < 0) {
      help.push(sum + data[i]);
    } else {
      help.push(sum);
    }
  }
}

option = {
  title: {
    text: 'Waterfall'
  },
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  xAxis: {
    type: 'category',
    splitLine: { show: false },
    data: (function() {
      var list = [];
      for (var i = 1; i <= 11; i++) {
        list.push('Oct/' + i);
      }
      return list;
    })()
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      type: 'bar',
      stack: 'all',
      itemStyle: {
        normal: {
          barBorderColor: 'rgba(0,0,0,0)',
          color: 'rgba(0,0,0,0)'
        },
        emphasis: {
          barBorderColor: 'rgba(0,0,0,0)',
          color: 'rgba(0,0,0,0)'
        }
      },
      data: help
    },
    {
      name: 'positive',
      type: 'bar',
      stack: 'all',
      data: positive
    },
    {
      name: 'negative',
      type: 'bar',
      stack: 'all',
      data: negative,
      itemStyle: {
        color: '#f33'
      }
    }
  ]
};
```

live
