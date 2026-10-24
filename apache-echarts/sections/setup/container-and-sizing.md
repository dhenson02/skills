# Container, Sizing, and Lifecycle

API: [`echarts.init`](https://echarts.apache.org/api.html#echarts.init), [`resize`](https://echarts.apache.org/api.html#echartsInstance.resize), [`dispose`](https://echarts.apache.org/api.html#echartsInstance.dispose).

## Initialization

`echarts.init(dom, theme?, opts?)`. Without `opts.width`/`opts.height`, the chart takes the container's size — so the container **must have non-zero width and height before `init`**. A zero-height `<div>` is the most common cause of a blank chart.

```html
<div id="main" style="width: 600px; height: 400px;"></div>
<script>
  const chart = echarts.init(document.getElementById('main'));
</script>
```

Explicit size, independent of the container:

```js
const chart = echarts.init(document.getElementById('main'), null, { width: 600, height: 400 });
```

Other useful `opts`: `renderer: 'canvas' | 'svg'`, `devicePixelRatio`, `locale`, `useCoarsePointer`, `pointerSize` (see [events-and-actions.md](../interaction/events-and-actions.md#pointer-snapping)).

## Responsive size

The chart does not track its container automatically. Call `resize()` when size changes.

```html
<style>
  #main { width: 100%; height: 400px; }
</style>
<div id="main"></div>
<script>
  const chart = echarts.init(document.getElementById('main'));
  window.addEventListener('resize', () => chart.resize());
</script>
```

`window` `resize` misses container-only changes (sidebar toggles, flex/grid reflow, tabs). Prefer [`ResizeObserver`](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver):

```js
const observer = new ResizeObserver(() => chart.resize());
observer.observe(document.getElementById('main'));
```

Resize to an explicit size — takes an object, **not** `resize(800, 400)`:

```js
chart.resize({ width: 800, height: 400 });
```

## Dispose and rebuild

An instance is bound to its DOM node. If the node is removed (tab switch, SPA route change, component unmount), the chart is gone even if equivalent markup is re-added.

- On removal: `chart.dispose()` (releases memory, listeners, timers).
- On re-insertion: `echarts.init(newNode)` again.

`echarts.getInstanceByDom(dom)` returns an existing instance, avoiding double init on the same node.
