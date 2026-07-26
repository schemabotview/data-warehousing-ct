## Choosing an SCD type per attribute

The most common mistake is thinking SCD is a *table* setting — "`DIM_CUSTOMER` is Type 2." It isn't. **SCD is chosen per attribute.** A single dimension routinely mixes types, column by column.

### One dimension, many policies

A realistic `DIM_CUSTOMER`:

- `date_of_birth` → **Type 0** — immutable; never changes.
- `name` (typo fix) → **Type 1** — overwrite; you don't want the wrong old value.
- `city` / `segment` → **Type 2** — full history; sales analysis depends on where/what they were at the time.
- `region` during a re-org → **Type 3** — dual old/new grouping for a transition.
- `income_band` → **Type 4** mini-dimension — changes too often to version the whole customer.

Each column is decided on its own merits; the ETL applies the right rule to each.

### The one question to ask per column

> *Do I need to know what this attribute was **at the time of the fact**?*

- **No, and old value is worthless** → **Type 1** (overwrite).
- **No — it never changes** → **Type 0** (retain original).
- **Yes — I need the full timeline** → **Type 2** (add a row).
- **Only current vs. one prior**, for dual reporting → **Type 3**.
- **Yes, but it changes too fast for Type 2** → **Type 4** mini-dimension.
- **Both as-was and as-is** from one place → **Type 6**.

### The quick reference

| Type | Strategy | History kept |
|------|----------|--------------|
| 0 | retain original | original only |
| 1 | overwrite | none (current only) |
| 2 | add a row | full timeline |
| 3 | add a column | current + one prior |
| 4 | history / mini-dim table | full, relocated |
| 6 | hybrid 1+2+3 | full + current on every row |

Weigh history value against cost: Type 2 is powerful but grows the table and complicates ETL; Type 1 is cheap but forgets. Match the type to how much the business will really ask "what was it back then" for *that* column.

> Decide SCD per attribute, not per table. Ask "do I need this value as-of the fact?" for each column, and pick the lightest type that answers the questions that column will actually face.
