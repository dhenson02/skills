# Labels: Options, Position, Rotation

`label` exists on most series (`bar`, `line`, `scatter`, `pie`, …), on axes (`axisLabel`), and in `emphasis`. Fragment styling (`rich`): [rich-text.md](rich-text.md).

## Text options

| Group | Options |
| --- | --- |
| Font | `fontStyle`, `fontWeight`, `fontSize`, `fontFamily` |
| Fill / stroke | `color`, `textBorderColor`, `textBorderWidth` |
| Text shadow | `textShadowColor`, `textShadowBlur`, `textShadowOffsetX`, `textShadowOffsetY` |
| Box size | `lineHeight`, `width`, `height`, `padding` (`width`/`height` apply with `rich` or `overflow`) |
| Alignment | `align`, `verticalAlign` |
| Box border/background | `backgroundColor` (color or `{ image }`), `borderColor`, `borderWidth`, `borderRadius` |
| Box shadow | `shadowColor`, `shadowBlur`, `shadowOffsetX`, `shadowOffsetY` |
| Placement | `position`, `distance`, `rotate`, `offset` |
| Content | `show`, `formatter` |

## formatter

String template or callback. Template vars (series-dependent): `{a}` series name, `{b}` data name, `{c}` value, `{d}` percent (pie), `{@dim}` / `{@[index]}` dataset dimension.

```js
label: { show: true, formatter: '{b}: {c}' }
label: { show: true, formatter: (params) => `${params.name}: ${params.value}` }
```

## Position and distance

```js
series: [{
  type: 'scatter',
  symbolSize: 160,
  symbol: 'roundRect',
  data: [[1, 1]],
  label: {
    show: true,
    // 'top' | 'left' | 'right' | 'bottom' | 'inside' | 'insideTop' | 'insideLeft'
    // | 'insideRight' | 'insideBottom' | 'insideTopLeft' | 'insideTopRight'
    // | 'insideBottomLeft' | 'insideBottomRight' | [x, y]
    position: 'top',
    distance: 10,
    formatter: 'Label Text',
    backgroundColor: '#eee',
    borderColor: '#555',
    borderWidth: 2,
    borderRadius: 5,
    padding: 10,
    fontSize: 18,
    color: '#fff',
    textBorderColor: '#000',
    textBorderWidth: 3
  }
}]
```

Valid `position` values differ by series (pie: `'outside' | 'inside' | 'center'`); `distance` isn't supported everywhere.

## Rotation

`align` / `verticalAlign` apply first, then `rotate`.

```js
const labelOption = {
  show: true,
  rotate: 90,
  align: 'left',
  verticalAlign: 'middle',
  position: 'insideBottom',
  distance: 15,
  formatter: '{c}  {name|{a}}',
  fontSize: 16,
  rich: { name: {} }
};

option = {
  xAxis: [{ type: 'category', data: ['2012', '2013', '2014', '2015', '2016'] }],
  yAxis: [{ type: 'value' }],
  series: [
    { name: 'Forest', type: 'bar', barGap: 0, label: labelOption, emphasis: { focus: 'series' }, data: [320, 332, 301, 334, 390] },
    { name: 'Steppe', type: 'bar', label: labelOption, emphasis: { focus: 'series' }, data: [220, 182, 191, 234, 290] }
  ]
};
```

Overlap handling: `labelLayout: { hideOverlap: true }` (needs `LabelLayout` feature in tree-shaken builds).
