# Rich Text Labels

Style fragments of a label independently (v3.7+). Works in any `label`/`axisLabel`/`title` text. Terms:

- **Text block** — the whole label; styled by top-level label options.
- **Text fragment** — a marked span inside it; styled by `rich.<styleName>`.

Mark fragments with `{styleName|text}`. Only `'\n'` breaks lines.

## Basic

```js
label: {
  formatter: [
    '{a|Style "a" applied}',
    '{b|Style "b" applied}default style{x|style "x"}'
  ].join('\n'),

  // text block
  color: '#333',
  fontSize: 5,
  fontFamily: 'Arial',
  borderWidth: 3,
  backgroundColor: '#984455',
  padding: [3, 10, 10, 5],
  lineHeight: 20,

  // fragments
  rich: {
    a: { color: 'red', lineHeight: 10 },
    b: { backgroundColor: { image: 'xxx/xxx.jpg' }, height: 40 },
    x: { fontSize: 18, fontFamily: 'Microsoft YaHei', borderColor: '#449933', borderRadius: 4 }
  }
}
```

Block and fragments both accept font, fill/stroke, text shadow, `borderColor`/`borderWidth`/`borderRadius`, `backgroundColor`, `padding`, and box shadow.

## Layout rules

Treat each fragment like a CSS `inline-block`.

- **Fragment size:** content box from font size (or explicit `width`/`height`) + `padding` = border box.
- **Line height:** the largest `lineHeight` among the line's fragments (from `rich`, the parent label, or the box size).
- **`verticalAlign`** within the line: `'top'`, `'middle'`, `'bottom'` (fragment edge sticks to line edge).
- **Block width:** `width` if set, else longest line.
- **Horizontal placement** per line: `align: 'left'` fragments packed from the left, then `'right'` fragments from the right, remainder centered in the leftover space.
- **Text inside a fragment** aligns by the fragment's own `align`.
- `width: '100%'` is relative to the block's content box (excludes padding).

## Effects

```js
rich: {
  // Icon: image background; set height only to keep aspect ratio
  sunny: {
    height: 30,
    align: 'left',
    backgroundColor: { image: 'https://echarts.apache.org/examples/data/asset/img/weather/sunny_128.png' }
  },

  // Horizontal rule
  hr: { borderColor: '#777', width: '100%', borderWidth: 0.5, height: 0 },

  // Title bar
  titleBg: {
    backgroundColor: '#000',
    height: 30,
    borderRadius: [5, 5, 0, 0],
    padding: [0, 10, 0, 10],
    width: '100%',
    color: '#eee'
  },
  tc: { align: 'center', color: '#eee' }
}

// Left title:     formatter: '{titleBg|Left Title}'
// Right title:    same, with rich.titleBg.align: 'right'
// Centered title: formatter: '{tc|Center Title}{titleBg|}' with rich.titleBg.align: 'right'
```

Card example combining them:

```js
formatter: [
  '{titleBg|Left Title}',
  '  Content text {sunny|} more text  ',
  '{hr|}',
  '  Footer text  '
].join('\n')
```

**Simple table:** give fragments in the same column equal `width` across lines. Example: [pie-rich-text](https://echarts.apache.org/examples/en/editor.html?c=pie-rich-text).
