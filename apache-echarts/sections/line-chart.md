# Line Chart

*Source: [Basic Line - Line - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/line/basic-line)*

## Basic Line Chart

### Simple Example

We can use the following code to build a line chart which has x-axis as `category`, y-axis as `value`:

```
option = {
  xAxis: {
    type: 'category',
    data: ['A', 'B', 'C']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      data: [120, 200, 150],
      type: 'line'
    }
  ]
};
```

live

In this case, we set the type of axis to `category` and `value` under `xAxis` and `yAxis`. We also clarified the content on the x-axis through `data`. In `series`, we set the type to `line`, and specify the values of three points through `data`. In this way, we got a simple line chart.

> The `type` here can be omitted because the defaults of the axis are `value` while `xAxis` has specified the category's `data`. In this case, `ECharts` can recognize that this is a category axis.

### Line Chart in Cartesian Coordinate System

How to implement if we want the line chart to be continuous? The answer is simple, as long as every value in `data` of the `series` is represented by an array containing two elements.

```
option = {
  xAxis: {},
  yAxis: {},
  series: [
    {
      data: [
        [20, 120],
        [50, 200],
        [40, 50]
      ],
      type: 'line'
    }
  ]
};
```

live

### Customized Line Chart

#### Line Style

Line style can be changed by `lineStyle` setting. You can specify color, line width, polyline type and opacity etc.. For details, please see the handbook [`series.lineStyle`](https://echarts.apache.org/option.html#series-line.lineStyle) to figure out.

Here is an example of setting color, line width and type.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 23, 19],
      type: 'line',
      lineStyle: {
        normal: {
          color: 'green',
          width: 4,
          type: 'dashed'
        }
      }
    }
  ]
};
```

live

When we set the line width here, the line width of items will not change. The line style of items needs to be set separately.

#### Item Style

Item style can be change by [`series.itemStyle`](https://echarts.apache.org/option.html#series-line.itemStyle). It included `color`, `borderColor`, `borderStyle`, `borderWidth`, `borderType`, `shadowColor`, `opacity` and so on. It works the same as the `lineType`, so we will not do further discuss.

### Display Value on Items.

In the series, the label of the item was specified by [`series.label`](https://echarts.apache.org/option.html#series-line.label). If we change the `show` under `label` to `true`, the value will be displayed by default. Otherwise, if [`series.emphasis.label.show`](https://echarts.apache.org/option.html#series-line.emphasis.label.show) is `true`, the label will show only if the mouse moved across the item.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 23, 19],
      type: 'line',
      label: {
        show: true,
        position: 'bottom',
        textStyle: {
          fontSize: 20
        }
      }
    }
  ]
};
```

live

### Empty Data

In a `series`, there are empty data. It has some difference with `0`. While there are empty elements, the lines chart will ignore that point without pass through it----empty elements will not be connected by the points next by.

In ECharts, we use `'-'` to represent null data, It is applicable for data in other series.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [0, 22, '-', 23, 19],
      type: 'line'
    }
  ]
};
```

live

> Please note the difference between the empty data and 0.

*Source: [Stacked Line - Line - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/line/stacked-line)*

## Stacked Line Chart

Similar to the [Stacked Bar Chart](https://echarts.apache.org/handbook/en/how-to/chart-types/bar/stacked-bar), Stacked Line Chart use the `'stack'` in `series` to decide which series should be stacked together.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 43, 49],
      type: 'line',
      stack: 'x'
    },
    {
      data: [5, 4, 3, 5, 10],
      type: 'line',
      stack: 'x'
    }
  ]
};
```

live

However, without clarification, it is hard for us to judge whether it is a stacked line chart or normal line chart. Therefore, filling color for the area under the line is recommended to indicate for a stacked line chart.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 43, 49],
      type: 'line',
      stack: 'x',
      areaStyle: {}
    },
    {
      data: [5, 4, 3, 5, 10],
      type: 'line',
      stack: 'x',
      areaStyle: {}
    }
  ]
};
```

live

*Source: [Area Chart - Line - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/line/area-line)*

## Area Chart

The area chart fills the space between the line and axis with the background color, to express the data by the size of the area. Compared with the normal line chart, the area chart has more intuitive visual effects. It is especially suitable for the scenario with a few series.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 23, 19],
      type: 'line',
      areaStyle: {}
    },
    {
      data: [25, 14, 23, 35, 10],
      type: 'line',
      areaStyle: {
        color: '#ff0',
        opacity: 0.5
      }
    }
  ]
};
```

live

If you want to change the area style of the line chart, try to use [`areaStyle`](https://echarts.apache.org/option.html#series-line.areaStyle). Set `'areaStyle'` to `{}` to use the default type: use the color of series to fill the area in translucent. If you want to change the style, try to override the configuration items in `'areaStyle'`. For example, the color of the second series was changed to yellow with 50% opacity.

*Source: [Smoothed Line - Line - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/line/smooth-line)*

## Smooth Line Chart

The smooth line chart is also a variant of the basic line graph. It is a better choice for you to perform a comfort visual experience. While using the ECharts, you only need to change the `smooth` to `true` to achieve this effect.

```
option = {
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 23, 19],
      type: 'line',
      smooth: true
    }
  ]
};
```

live

*Source: [Step Line - Line - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/line/step-line)*

## Step Line Chart

The normal line chart connects two points by straight line directly, while the step line chart, also known as square wave chart, uses only horizontal and vertical lines to connect the nearby items together. Compared with the normal line chart, the step line chart significantly shows the sudden changes of analyzed data.

In ECharts, `step` is used to characterize the connection type of the step line chart. It has three available values: `'start'`, `'end'`, and `'middle'`. For item A and B, these values corresponded that the corner occurred at A, B, and mid-point between A and B.

```
option = {
  legend: {},
  xAxis: {
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      name: 'Step Start',
      type: 'line',
      step: 'start',
      data: [120, 132, 101, 134, 90, 230, 210]
    },
    {
      name: 'Step Middle',
      type: 'line',
      step: 'middle',
      data: [220, 282, 201, 234, 290, 430, 410]
    },
    {
      name: 'Step End',
      type: 'line',
      step: 'end',
      data: [450, 432, 401, 454, 590, 530, 510]
    }
  ]
};
```

live

> Please focus on the difference of line between three separate types.
