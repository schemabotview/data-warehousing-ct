## Type 4 — history / mini-dimension tables

Types 1–3 all keep the change *inside* the one dimension. **Type 4** takes a different route: **move the changing part into a separate table**. It comes in two practical flavours.

### Flavour A — the history table

Keep the main dimension **current-only** (Type 1, one row per entity, always the latest), and push every past version into a **companion history table**:

```
DIM_CUSTOMER          — current row only (fast, small)
DIM_CUSTOMER_HISTORY  — every past version, with effective/expiry dates
```

Day-to-day queries hit the lean current table; the occasional "what was it back then" query goes to the history table. It's Type 2's history, physically separated from the hot current record.

### Flavour B — the mini-dimension

Some attributes change **too fast** for Type 2 — every change spawns a new row, and a big dimension bloats. The fix: pull those **volatile, band-able attributes** out into a small **mini-dimension** of their own, which the **fact references directly**.

For a customer, the fast-moving demographics — `age_band`, `income_band`, `credit_score_band` — leave `DIM_CUSTOMER` and become `DIM_DEMOGRAPHICS`, a small table of every distinct *combination* of bands. The fact then carries **two** keys:

```
FACT_SALES.customer_key       ──►  DIM_CUSTOMER      (stable attributes)
FACT_SALES.demographics_key   ──►  DIM_DEMOGRAPHICS  (volatile bands)
```

When a customer's income band changes, **no new customer row** is created — the fact simply starts pointing at a different `demographics_key`. History is captured by which mini-dimension key each fact used, and the big dimension stays stable.

### When to reach for Type 4

- **History table** — you want a small, fast current dimension but still need full history somewhere.
- **Mini-dimension** — a *subset* of attributes changes so often that Type 2 would explode the dimension's row count.

> Type 4 relocates change: a history table to keep the current dimension lean, or a mini-dimension to hold fast-changing bands the fact points at directly — so the main dimension doesn't bloat.
