# Security

ECharts assumes trusted input and does not sanitize. Most options are rendered as inert text on canvas/SVG, but the APIs below inject raw HTML, CSS, URLs, filenames, regexes, or code. Treat any value derived from users, external APIs, or stored configs as untrusted.

Report vulnerabilities: [ASF security page](https://echarts.apache.org/en/security.html).

## Checklist

| API | Risk | Mitigation |
| --- | --- | --- |
| `tooltip.formatter` returning HTML string / DOM | XSS | Escape interpolated data (string templates like `'{b}: {c}'` and `renderMode: 'richText'` are safe) |
| `toolbox.feature.dataView.optionToContent`, `.title`, `.lang` | XSS (rendered as HTML) | Escape; sanitize/sandbox if markup is untrusted |
| `tooltip.extraCssText` | CSS injection | Only trusted CSS |
| `title.link`, `title.sublink`, `series-treemap.data.link`, `series-sunburst.data.link` | `javascript:` / `data:` URLs | Allowlist protocols (`http:`, `https:`) |
| `toolbox.feature.saveAsImage.name` / `.type` (falls back to `title[0].text`) | Unsafe filenames | Restrict to short ASCII alphanumerics |
| `dataset.transform` filter `config.reg` | ReDoS — freezes tab or SSR event loop | Don't accept untrusted patterns |
| Any function option (formatters, callbacks) | Code execution | Never build from untrusted strings |

## HTML escaping

Always escape data before putting it in HTML — for correctness as well as safety.

```js
// Unsafe: name/value may contain markup
formatter: (params) => `${params.name}, <b>${params.value}</b>`

// Safe
formatter: (params) => {
  const { encodeHTML } = echarts.format;
  return `${encodeHTML(params.name)}, <b>${encodeHTML(String(params.value))}</b>`;
}
```

Escaping `& < > " '` suffices when only data is untrusted and markup is yours. Alternatively build DOM nodes and set `.textContent`.

## Untrusted markup

When untrusted content must render as HTML (e.g. user-authored `<em>`, `<a>`):

- Use a well-maintained sanitizer (e.g. DOMPurify), never hand-rolled regex. Strip `<script>`, `<style>`, `<link>`, inline CSS, `on*` handlers, `javascript:`/`data:` URLs.
- Sanitize **server-side** before persisting — client-side checks are bypassable.
- Prefer storing **IDs of built-in templates/formatters** over raw HTML or function source from clients.
- If executing untrusted code is unavoidable, isolate in a sandboxed iframe.

## Regex in dataset filter

String `reg` values compile straight to `RegExp` and run against every row with no limits. Patterns like `^(a+)+$` cause catastrophic backtracking.

If configs can come from users (saved dashboards, APIs):

- Disallow `reg` from external config; define patterns in code, or
- Enforce max length and reject nested quantifiers / allowlist simple patterns, or
- Use a linear-time engine (RE2) server-side, or run matching in a Worker with a timeout.

## Function options

Callbacks in the application's own code are safe. Loading function source from users is equivalent to allowing arbitrary script — avoid, or sandbox.
