## Attributes & hierarchies

A dimension earns its keep through its **attributes** — the descriptive columns — and the **hierarchies** they form. Together they decide how richly you can slice the data.

### Attributes — the columns you slice by

An **attribute** is one descriptive column of a dimension: `category`, `brand`, `color`, `city`, `segment`. Each is a lever for analysis — a way to **filter** ("only Headsets"), **group** ("by segment"), or **label** ("Madrid" on the axis). The rule of thumb is generosity: **more attributes = richer analysis**. A dimension with twenty good attributes answers twenty kinds of question; a thin one with three answers three. Storage is cheap; unanswerable questions are expensive.

### Hierarchies — the natural roll-up paths

Many attributes nest into a **hierarchy** — a many-to-one drill path from fine to coarse. `DIM_PRODUCT` has:

```
product → category → product_line → brand
```

and `DIM_DATE` has the universal one:

```
day → month → quarter → year
```

`DIM_CUSTOMER` carries a geography hierarchy — `city → province → region → country`. Hierarchies are what let a user **drill down** (year → quarter → month) and **roll up** (city → region) inside a single dimension. A report starts at "sales by year", the user clicks 2026, and the same hierarchy expands it to quarters.

### Keep the hierarchy flat (denormalized)

In a star schema the whole hierarchy lives as **flat columns in one dimension table** — `category`, `product_line`, and `brand` are all just columns on `DIM_PRODUCT`, repeated on every row. You do *not* split them into separate `category` and `brand` tables. That repetition is deliberate: it keeps every drill path one hop from the fact, so a query never chains joins. Normalising the hierarchy into sub-tables is the **snowflake** — a different trade-off, covered in module 05.

> Be generous with attributes, and store each hierarchy as flat columns on the one dimension. Rich, denormalized dimensions are what make BI fast *and* expressive.
