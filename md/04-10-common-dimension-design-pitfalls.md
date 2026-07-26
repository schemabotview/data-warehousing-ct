## Common dimension design pitfalls

Dimensions are where a model most often goes quietly wrong — not with a crash, but with reports that are hard to build or subtly untrustworthy. A checklist of the usual traps.

### The pitfalls, and the fixes

- **Too few attributes** — a thin dimension can only answer a few questions. Be **generous and descriptive**: extra attributes are cheap storage and future analysis you didn't have to predict.
- **Cryptic codes instead of labels** — storing `2` or `RTN` forces every report to decode it. Store **human-readable text** (`Returned`, `B2B`) so labels come straight off the dimension.
- **No surrogate key** — building on the source's natural key ties you to its numbering and **blocks SCD history**. Give every dimension a surrogate `_key`, natural key beside it.
- **NULL foreign keys** — a missing lookup left as NULL silently drops rows from joins. Use an **"Unknown" member** (key `0`) so every fact points at a real row.
- **Premature snowflaking** — normalising a hierarchy into sub-tables (`category`, `brand`) to save space adds joins and slows BI. **Keep dimensions flat and denormalized** (the trade-off is module 05).
- **Ignoring change over time** — pretending attributes never change loses history. Decide an **SCD policy per attribute** — overwrite or keep history — up front (module 06).
- **Measures hiding in a dimension** — a number you *aggregate* belongs on the fact, not as a dimension attribute. Apply the *"sum it or slice by it?"* test.

### The through-line

Most of these share one root: treating the dimension as an afterthought. The fact gets the design attention; the dimension gets whatever the source happened to provide.

> Invest in dimensions. Rich, labelled, surrogate-keyed, flat, and history-aware — that's what makes a warehouse pleasant to query and trustworthy to report from.
