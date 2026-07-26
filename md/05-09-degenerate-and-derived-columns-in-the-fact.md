## Degenerate & derived columns in the fact

A fact row is mostly foreign keys and plain measures — but two other kinds of column show up on it, and both are deliberate design decisions worth naming: **degenerate dimensions** and **derived columns**.

### Degenerate dimensions — a recap in place

A **degenerate dimension** is a dimension value stored on the fact with **no dimension table** — an operational identifier whose every descriptive attribute has already become its own dimension. On `FACT_SALES`: `order_id`, `order_line_id`, `order_status`. They aren't measures (you'd never sum them) and don't deserve a table (nothing left to describe), so they sit on the fact — earning their keep by **grouping a transaction's lines** (`GROUP BY order_id`) and providing an **audit trail** to the source.

### Derived columns — pre-computed on the fact

A **derived (computed) column** is a value calculated during ETL and **stored** on the fact rather than computed at query time. The Jabra fact's `line_total` is the classic example:

```
line_total = quantity * unit_price - discount_amount + tax_amount
```

Flags like `is_returned` are derived too. Storing them is a **trade-off**:

- **Store it (materialize)** — every query gets the same number instantly, with one agreed definition. Faster reads, guaranteed consistency. Costs a little space and ETL work.
- **Compute at query time** — no storage, always in sync with the inputs, but each query repeats the work and risks *different analysts computing it differently*.

Warehouses usually **store** common derived measures — speed and a single definition matter more than the space.

### One caution — keep it additive

Store derived **amounts** (additive), not derived **ratios**. Recall the non-additive rule: don't pre-compute `discount_pct` on the fact; store `discount_amount` and `line_total` and divide *after* aggregating. A stored ratio can't be summed and will mislead.

> Beyond keys and measures, a fact carries **degenerate dimensions** (ids with no table, for grouping and audit) and **derived columns** (pre-computed measures like `line_total`) — materialized for speed and one definition, but kept additive.
