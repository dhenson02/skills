# Pie Chart

*Source: [Basic Pie - Pie - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/pie/basic-pie)*

## Basic Pie Chart

Pie charts are mainly used to show the proportion of several categories compared with the total. The radians represent the proportion of each category.

### Simple Example

The config of the pie chart is not completely the same as the line chart and bar chart. There is no need to configure the axis. The name and value of data need to be defined in the series. Let's begin with a basic pie chart:

```
option = {
  series: [
    {
      type: 'pie',
      data: [
        {
          value: 335,
          name: 'Direct Visit'
        },
        {
          value: 234,
          name: 'Union Ad'
        },
        {
          value: 1548,
          name: 'Search Engine'
        }
      ]
    }
  ]
};
```

live

To be mentioned, the `value` here does not need to be percentage data. ECharts will proportionately distribute their corresponding radians in the pie chart depending on all the data.

### Customized Pie Chart

#### Radius of Pie Chart

The radius of pie chart can be defined by [`series.radius`](https://echarts.apache.org/option.html#series-pie.radius). Both percent string(`'60%'`) and absolute pixel string(`'200'`) are available. While it is a percent string, it is proportional related to the shorter container(`'div'`) edge.

```
option = {
  series: [
    {
      type: 'pie',
      data: [
        {
          value: 335,
          name: 'Direct Visit'
        },
        {
          value: 234,
          name: 'Union Ad'
        },
        {
          value: 1548,
          name: 'Search Engine'
        }
      ],
      radius: '50%'
    }
  ]
};
```

live

### Hide Chart While Data Sum is 0

By default, if the data sum is 0, the series will divide the shape equally. For instance, if you don't want to show any shape while all 4 series have value equals 0, you could define [`series.stillShowZeroSum`](https://echarts.apache.org/option.html#series-pie.stillShowZeroSum) to `false`.

```
option = {
  series: [
    {
      type: 'pie',
      stillShowZeroSum: false,
      data: [
        {
          value: 0,
          name: 'Direct Visit'
        },
        {
          value: 0,
          name: 'Union Ad'
        },
        {
          value: 0,
          name: 'Search Engine'
        }
      ]
    }
  ]
};
```

live

If you are willing to hide the label as well, define the [`series.label.show`](https://echarts.apache.org/option.html#series-pie.label.show) to `false` as well.

```
option = {
    series: [{
        type: 'pie',
        stillShowZeroSum: false,
        label: {
            show: false
        }
        data: [{
            value: 0,
            name: 'Direct Visit'
        }, {
            value: 0,
            name: 'Union Ad'
        }, {
            value: 0,
            name: 'Search Engine'
        }]
    }]
};
```

live

*Source: [Ring Style - Pie - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/pie/doughnut)*

## Doughnut Chart

Doughnut charts are also used to show the proportion of values compared with the total. Different from the pie chart, the blank in the middle of the chart can be used to provide some extra info. It makes a doughnut chart commonly used chart type.

### Basic Doughnut Chart

In ECharts, the radius of the pie chart could also be an array with 2 elements. Every element in the array could be string or value. The first element represents the inner radius while the second one is the outer radius.

The pie chart, from this perspective, is a subset of the doughnut chart that has inner radius equals to 0.

```
option = {
  title: {
    text: 'A Case of Doughnut Chart',
    left: 'center',
    top: 'center'
  },
  series: [
    {
      type: 'pie',
      data: [
        {
          value: 335,
          name: 'A'
        },
        {
          value: 234,
          name: 'B'
        },
        {
          value: 1548,
          name: 'C'
        }
      ],
      radius: ['40%', '70%']
    }
  ]
};
```

live

If we set one radius to string of a percentage value, while the other to a value, the inner radius will be smaller than the outer radius in some resolution. ECharts will choose the smaller element for the inner radius automatically. However, it will still cause not an unexpected outcome.

### Show Text In Middle Of Doughnut From Highlighted Sector

The previous case gives you a way to show fixed text in the middle of doughnut chart. The next case will show you how to display the corresponding text of the sector highlighted by the mouse. The general idea is to fix `label` in the middle of the chart while hiding `label` in default.

```
option = {
  legend: {
    orient: 'vertical',
    x: 'left',
    data: ['A', 'B', 'C', 'D', 'E']
  },
  series: [
    {
      type: 'pie',
      radius: ['50%', '70%'],
      avoidLabelOverlap: false,
      label: {
        show: false,
        position: 'center'
      },
      labelLine: {
        show: false
      },
      emphasis: {
        label: {
          show: true,
          fontSize: '30',
          fontWeight: 'bold'
        }
      },
      data: [
        { value: 335, name: 'A' },
        { value: 310, name: 'B' },
        { value: 234, name: 'C' },
        { value: 135, name: 'D' },
        { value: 1548, name: 'E' }
      ]
    }
  ]
};
```

live

In this case, `avoidLabelOverlap` is used to control whether ECharts adjust the position of label to avoid overlaps. Default value of `avoidLabelOverlap` is `true`. We want the label to be fixed in the middle so that we need to define it as `false`.

Therefore, the middle of doughnut chart will show the `name` of the highlighted sector.

*Source: [Rose Style - Pie - Common Charts - How To Guides - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/how-to/chart-types/pie/rose)*

## Rose Chart（Nightingale Chart）

Rose Chart, which was also called the nightingale chart, usually indicates categories by sector of the same radius but different radius.

ECharts can implement Rose Chart by defining [`series.roseType`](https://echarts.apache.org/option.html#series-pie.roseType) of pie chart to `'area'`. All other configs are the same as a basic pie chart.

```
option = {
  series: [
    {
      type: 'pie',
      data: [
        {
          value: 100,
          name: 'A'
        },
        {
          value: 200,
          name: 'B'
        },
        {
          value: 300,
          name: 'C'
        },
        {
          value: 400,
          name: 'D'
        },
        {
          value: 500,
          name: 'E'
        }
      ],
      roseType: 'area'
    }
  ]
};
```

live
