---
name: apache-echarts
description: "Use when creating data visualizations, charts, graphs, etc."
---

# apache-echarts

Use when creating data visualizations, charts, graphs, etc.

*Source: [Get Started - Handbook - Apache ECharts](https://echarts.apache.org/handbook/en/get-started)*

## Get Started

### Getting Apache ECharts

Apache ECharts supports several Installation methods, which are explained in detail in the next [Installation](https://echarts.apache.org/handbook/en/basics/download) tutorial. Here, we'll use [jsDelivr](https://www.jsdelivr.com/package/npm/echarts) CDN as an example to quickly set up ECharts.

At [https://www.jsdelivr.com/package/npm/echarts](https://www.jsdelivr.com/package/npm/echarts) select `dist/echarts.js`, click and save it as `echarts.js` file.

> More information about these files can be found in the next tutorial [Installation](https://echarts.apache.org/handbook/en/basics/download).

### Including ECharts

Create a new `index.html` file in the directory where you just saved `echarts.js`, with the following content:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <!-- Include the ECharts file you just downloaded -->
    <script src="echarts.js"></script>
  </head>
</html>
```

When you open `index.html` the page will appear blank. But don't worry. Open the console and make sure that no error message is reported, then you can proceed to the next step.

### Plotting a Simple Chart

Before rendering a chart we need to prepare a DOM container for ECharts with a defined height and width. Add the following code immediately after the closing `</head>` tag.

```html
<body>
  <!-- Prepare a DOM with a defined width and height for ECharts -->
  <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

Next, initialize an ECharts instance using [echarts.init](https://echarts.apache.org/api.html#echarts.init) method and configure it with [setOption](https://echarts.apache.org/api.html#echartsInstance.setOption) method to generate a simple bar chart. Here is the complete code.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>ECharts</title>
    <!-- Include the ECharts file you just downloaded -->
    <script src="echarts.js"></script>
  </head>
  <body>
    <!-- Prepare a DOM with a defined width and height for ECharts -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
      // Initialize the echarts instance based on the prepared dom
      var myChart = echarts.init(document.getElementById('main'));

      // Specify the configuration items and data for the chart
      var option = {
        title: {
          text: 'ECharts Getting Started Example'
        },
        tooltip: {},
        legend: {
          data: ['sales']
        },
        xAxis: {
          data: ['Shirts', 'Cardigans', 'Chiffons', 'Pants', 'Heels', 'Socks']
        },
        yAxis: {},
        series: [
          {
            name: 'sales',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      };

      // Display the chart using the configuration and the data defined above.
      myChart.setOption(option);
    </script>
  </body>
</html>
```

And this is your first chart with Apache ECharts!

## Reference files

Detailed documentation is split by topic. Read only the files relevant to the task.

- [Installation](sections/installation.md) — covers: Using ECharts as an NPM Package; Install ECharts via NPM; Import All ECharts Functionality; Shrinking Bundle Size
- [Chart Container](sections/chart-container.md) — covers: Chart Container and Size; Initialization; Define a Parent Container in HTML; Specify the chart size
- [Style](sections/style.md) — covers: Overview of Style Customization; Theme; Color Palette; Customize Style Explicitly
- [Dataset](sections/dataset.md) — covers: Define data under series; Define data in dataset; Map from Data to Chart; Map Row or Column of dataset to series
- [Data Transform](sections/data-transform.md) — covers: Get Started to Data Transform; Advanced Usage; Filter Transform; Sort Transform
- [Axis](sections/axis.md) — covers: x-axis, y-axis; Axis Line; Tick; Label
- [Visual Mapping](sections/visual-mapping.md) — covers: Visual Map of Data; Data and Dimension; The visualMap Component; Continuous and Piecewise Visual Mapping Components
- [Legend](sections/legend.md) — covers: Layout; Style; Interactive; Tips
- [Event and Action](sections/event-and-action.md) — covers: Handling the Mouse Events; Event of Component Interaction; Writing Code to Trigger Component Action Manually; Listen to Events on the Blank Area
- [Bar Chart](sections/bar-chart.md) — covers: Basic Bar Chart; Simple Example; Multi-series Bar Chart; Customized Bar Chart
- [Line Chart](sections/line-chart.md) — covers: Basic Line Chart; Simple Example; Line Chart in Cartesian Coordinate System; Customized Line Chart
- [Pie Chart](sections/pie-chart.md) — covers: Basic Pie Chart; Simple Example; Customized Pie Chart; Radius of Pie Chart
- [Scatter Chart](sections/scatter-chart.md) — covers: Basic Scatter Chart; Simple Example; Scatter Chart in Cartesian Coordinate System; Customized Scatter Chart
- [Series Data](sections/series-data.md) — covers: Custom Series; Registerable Custom Series (New); Using Published Custom Series; Developing Your Own Custom Series
- [Geo/Map](sections/geo-map.md) — covers: SVG Base Map; Basic Usage; Zoom and Pan; Named Element
- [Server-side Rendering (SSR)](sections/server-side-rendering-ssr.md) — covers: Server-Side Rendering; Server-Side SVG Rendering; Server-Side Canvas Rendering; Client Hydration
- [Data Loading](sections/data-loading.md) — covers: Asynchronous Data Loading and Dynamic Update; Asynchronous Loading; loading Animation; Dynamic Update
- [Label](sections/label.md) — covers: Rich Text; Options about Text; Basic Styles of Text, Text Block and Text Fragment; Label Position
- [Animation](sections/animation.md) — covers: Data Transition; Configuration of Transitions; Animation Duration; Animation Easing
- [Interaction](sections/interaction.md) — covers: An Example: Implement Dragging; Implement basic dragging; Add tooltip component; Full code
- [Best Practices](sections/best-practices.md) — covers: Render with SVG or Canvas; How to Choose a Renderer; How to Use the Renderer; Web Accessibility
