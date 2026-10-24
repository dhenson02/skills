# Maps: GeoJSON and SVG Base Maps

Two entry points share the same map options:

- [`geo`](https://echarts.apache.org/option.html#geo) component — a coordinate system; other series (`scatter`, `effectScatter`, `lines`, `custom`) plot on it with `coordinateSystem: 'geo'`.
- [`series-map`](https://echarts.apache.org/option.html#series-map) — a choropleth series; pairs with `visualMap` for coloring.

ECharts bundles no map data. Register a map by name first with [`echarts.registerMap`](https://echarts.apache.org/api.html#echarts.registerMap). Layout details and SVG limits: [geo-svg-advanced.md](geo-svg-advanced.md).

## GeoJSON

```js
const geoJson = await fetch('usa.json').then((r) => r.json());
echarts.registerMap('USA', geoJson);

chart.setOption({
  tooltip: { trigger: 'item' },
  visualMap: { min: 0, max: 40000000, inRange: { color: ['#e0f3f8', '#08589e'] } },
  series: [{
    type: 'map',
    map: 'USA',
    roam: true,
    data: [{ name: 'California', value: 39500000 }, { name: 'Texas', value: 29100000 }]  // name = feature properties.name
  }]
});
```

Scatter on geo takes `[lng, lat, ...]` data.

## SVG base map (v5.1+)

Any SVG (floor plan, anatomy, seat map) can serve as a map in both canvas and SVG renderers. Examples: [Beef Cuts](https://echarts.apache.org/examples/en/editor.html?c=geo-beef-cuts), [Organ](https://echarts.apache.org/examples/en/editor.html?c=geo-organ), [Flight Seatmap](https://echarts.apache.org/examples/en/editor.html?c=geo-seatmap-flight), [SVG Map](https://echarts.apache.org/examples/en/editor.html?c=geo-svg-map), [SVG Scatter](https://echarts.apache.org/examples/en/editor.html?c=geo-svg-scatter-simple), [SVG Lines](https://echarts.apache.org/examples/en/editor.html?c=geo-svg-lines).

```js
const svg = await fetch('map/organ.svg').then((r) => r.text());
echarts.registerMap('organ_diagram', { svg });   // raw string or parsed SVG DOM

chart.setOption({ geo: { map: 'organ_diagram' } });
// or: series: { type: 'map', map: 'organ_diagram' }
```

### Named elements

Interaction (select, emphasis, focus-blur, label, tooltip, events) applies only to SVG elements with a `name` attribute:

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <path name="named_rect" d="M 0,0 L 0,100 100,100 100,0 Z" fill="#765" />  <!-- interactive -->
  <path d="M 150,0 L 150,100 250,100 250,0 Z" fill="#567" />                 <!-- static -->
</svg>
```

- Nameable: `rect`, `circle`, `line`, `ellipse`, `polygon`, `polyline`, `path`, `text`, `tspan`, `g`.
- Styleable via `itemStyle`: the same minus `text`, `tspan`, `g`.
- Labelable via `label`: the styleable set plus `g`.
- Elements sharing a name highlight/select together.

## Common options (GeoJSON and SVG)

```js
geo: {
  map: 'some_map',
  roam: true,                         // zoom + pan ('scale' | 'move' for one)
  selectedMode: 'multiple',           // 'single' | 'multiple'
  itemStyle: { /* normal */ },
  emphasis: {
    focus: 'self',                    // blur everything else on hover
    itemStyle: { color: null },       // null = keep original fill on hover
    label: { show: false }            // hover labels are on by default
  },
  select: { itemStyle: { /* ... */ } },
  tooltip: { show: true },            // also needs root-level `tooltip: {}`
  regions: [                          // per-region overrides by name
    { name: 'element_name_1', itemStyle: { /* ... */ }, tooltip: { formatter: 'Special 1' } }
  ]
}
```

State styles available at both levels: `itemStyle`, `emphasis.itemStyle`, `select.itemStyle`, `blur.itemStyle`, and the same under `regions[i]`.

Shared label for several elements: wrap them in a named `<g>` (one label at the group's bounding box) or give them the same name (one label each).

## Events

```js
chart.on('click', { geoIndex: 0, name: 'name1' }, (params) => console.log(params));

chart.on('geoselectchanged', (params) => {
  console.log('selected', params.allSelected[0].name);
});
```
