# Built-in Transforms: filter and sort

Context: [transform.md](transform.md). Tree-shaken builds need `DatasetComponent` + `TransformComponent`.

## Contents
- filter: basic, operators, logic, parsers, type definition
- sort: basic, multi-key, rules, type definition

## filter

```js
option = {
  dataset: [
    {
      source: [
        ['Product', 'Sales', 'Price', 'Year'],
        ['Cake', 123, 32, 2011],
        ['Latte', 231, 14, 2011],
        ['Tofu', 235, 5, 2011],
        ['Cake', 143, 30, 2012],
        ['Latte', 201, 19, 2012]
      ]
    },
    { transform: { type: 'filter', config: { dimension: 'Year', '=': 2011 } } }
  ],
  series: { type: 'pie', datasetIndex: 1 }
};
```

### dimension

Dimension name (`dimension: 'Year'`) or 0-based index (`dimension: 3`).

### Relational operators

`>` (`gt`), `>=` (`gte`), `<` (`lt`), `<=` (`lte`), `=` (`eq`), `!=` (`ne`, `<>`), `reg`.

- Several operators in one object are ANDed: `{ dimension: 'Price', '>=': 20, '<': 30 }`.
- Numeric strings (`' 123 '`) are compared as numbers (whitespace trimmed).
- Pure string comparison works only with `=` / `!=`.
- Dates (JS `Date` or `'2012-05-12'`) need `parser: 'time'`.
- `reg` tests a RegExp: `{ dimension: 'Name', reg: /\s+Müller\s*$/ }`. String patterns are compiled with no complexity limit — never pass untrusted patterns (ReDoS; see [security.md](../best-practices/security.md#regex-in-dataset-filter)).

### Logical operators

`and` / `or` take arrays; `not` takes a single object. They nest.

```js
config: {
  and: [
    { dimension: 'Year', '=': 2011 },
    { dimension: 'Price', '>=': 20, '<': 30 }
  ]
}

config: {
  or: [
    { and: [{ dimension: 'Price', '>=': 10, '<': 20 }, { dimension: 'Sales', '<': 100 }, { not: { dimension: 'Product', '=': 'Tofu' } }] },
    { and: [{ dimension: 'Price', '>=': 10, '<': 20 }, { dimension: 'Sales', '<': 100 }, { not: { dimension: 'Product', '=': 'Cake' } }] }
  ]
}
```

### Parsers

- `'time'` — parse to timestamp (same rules as `echarts.time.parse`: `Date`, ms number, `'2012-05-12 03:11:22'`); otherwise `NaN`.
- `'trim'` — trim strings before comparing.
- `'number'` — loose numeric conversion, for values with units like `'33%'`, `'12px'`.

```js
{
  transform: {
    type: 'filter',
    config: { dimension: 'Date', '>=': '2012-05', '<': '2012-06', parser: 'time' }
  }
}
```

### Type definition

```ts
type FilterTransform = {
  type: 'filter';
  config: ConditionalExpressionOption;
};
type ConditionalExpressionOption =
  | true
  | false
  | RelationalExpressionOption
  | LogicalExpressionOption;
type RelationalExpressionOption = {
  dimension: DimensionName | DimensionIndex;
  parser?: 'time' | 'trim' | 'number';
  lt?: DataValue;  lte?: DataValue;  gt?: DataValue;  gte?: DataValue;
  eq?: DataValue;  ne?: DataValue;
  '<'?: DataValue; '<='?: DataValue; '>'?: DataValue; '>='?: DataValue;
  '='?: DataValue; '!='?: DataValue; '<>'?: DataValue;
  reg?: RegExp | string;
};
type LogicalExpressionOption = {
  and?: ConditionalExpressionOption[];
  or?: ConditionalExpressionOption[];
  not?: ConditionalExpressionOption;
};
type DataValue = string | number | Date;
type DimensionName = string;
type DimensionIndex = number;
```

## sort

```js
option = {
  dataset: [
    {
      dimensions: ['name', 'age', 'profession', 'score', 'date'],
      source: [
        [' Hannah Krause ', 41, 'Engineer', 314, '2011-02-12'],
        ['Zhao Qian ', 20, 'Teacher', 351, '2011-03-01'],
        [' Jasmin Krause ', 52, 'Musician', 287, '2011-02-14'],
        ['Li Lei', 37, 'Teacher', 219, '2011-02-18'],
        [' Adrian Groß', 19, 'Teacher', null, '2011-01-16']
      ]
    },
    { transform: { type: 'sort', config: { dimension: 'score', order: 'asc' } } }
  ],
  series: { type: 'bar', datasetIndex: 1 }
};
```

Multi-key — pass an array, applied in priority order:

```js
config: [
  { dimension: 'profession', order: 'desc' },
  { dimension: 'score', order: 'desc' }
]
```

### Rules

- Numbers and numeric strings sort numerically.
- Non-numeric strings sort among themselves (useful for grouping by tag in multi-key sorts).
- Mixed/incomparable values (`null`, `undefined`, `NaN`, `''`, `'-'`) go to the head or tail per `incomparable: 'min' | 'max'`.
- Parsers as in filter: `parser: 'time'` for dates, `parser: 'number'` for `'33%'`-style values.

### Type definition

```ts
type SortTransform = {
  type: 'sort';
  config: OrderExpression | OrderExpression[];
};
type OrderExpression = {
  dimension: DimensionName | DimensionIndex;
  order: 'asc' | 'desc';
  incomparable?: 'min' | 'max';
  parser?: 'time' | 'trim' | 'number';
};
```
