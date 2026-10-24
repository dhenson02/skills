# Server-Side Rendering

Use for fast first paint, or output where scripts can't run (Markdown, PDF, email, reports). Adding interactivity afterward: [hydration.md](hydration.md).

| Method | Output | Pros |
| --- | --- | --- |
| SVG (v5.3+, zero deps) — **preferred** | SVG string | Smaller, sharp at any zoom, CSS initial animation |
| Canvas via node-canvas | PNG/JPEG buffer | Works where SVG isn't accepted |

Lost in SSR: tooltips, legend toggling, dynamic data, other interaction.

## SVG

```js
const echarts = require('echarts');

let chart = echarts.init(null, null, {   // no DOM container
  renderer: 'svg',                       // required
  ssr: true,                             // disables animation loop and event modules
  width: 400,                            // required — no container to measure
  height: 300
});

chart.setOption({ /* ... */ });

const svgStr = chart.renderToSVGString();

chart.dispose();   // free memory
chart = null;
```

Serve or save:

```js
res.writeHead(200, { 'Content-Type': 'image/svg+xml' });
res.end(svgStr);

fs.writeFileSync('bar.svg', svgStr, 'utf-8');
```

Fixed width/height means responsive sizing needs a re-render per size — consider whether SSR fits.

### Animation

The initial animation is embedded as CSS in the SVG (no JS). Not supported: bar race, label value animation, `lines` effects. Disable with `setOption({ animation: false })`.

## Canvas (node-canvas)

```js
const echarts = require('echarts');
const { createCanvas } = require('canvas');

const canvas = createCanvas(800, 600);
let chart = echarts.init(canvas);        // node-canvas instance as container
chart.setOption({ /* ... */ });

const buffer = canvas.toBuffer('image/png');
chart.dispose();
chart = null;

res.writeHead(200, { 'Content-Type': 'image/png' });
res.end(buffer);
```

Before v5.3, also call `echarts.setCanvasCreator(() => createCanvas())`.

### Images

For charts using images, adapt loading with `setPlatformAPI` (v5.3+):

```js
const { createCanvas, Image } = require('canvas');

echarts.setPlatformAPI({
  createCanvas() {
    return createCanvas();
  },
  loadImage(src, onload, onerror) {
    const img = new Image();
    img.onload = onload.bind(img);    // must bind to img
    img.onerror = onerror.bind(img);
    img.src = src;
    return img;
  }
});
```

Prefetch remote images and pass them as base64 data URLs so they're loaded at render time.

## Security

Server-rendered options built from user input: see [security.md](../best-practices/security.md) — a malicious regex in `dataset` filter `reg` can block the Node.js event loop.
