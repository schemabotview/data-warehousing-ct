## Composite keys

Sometimes no single column identifies a row — you need **two or more columns together**. A **composite key** is a primary key made of multiple columns that are unique only in combination.

### When one column isn't enough

Take an `ORDER_LINE` table recording each line of each order:

| order_id | line_no | product_id | qty |
|----------|---------|------------|-----|
| 1001 | 1 | P-77 | 2 |
| 1001 | 2 | P-88 | 1 |
| 1002 | 1 | P-77 | 5 |

`order_id` repeats across lines. `line_no` repeats across orders. Neither alone is unique — but `(order_id, line_no)` together identifies exactly one line. That pair is the composite primary key.

### Where composite keys show up

- **Bridge / junction tables** — modeling a many-to-many relationship (e.g. `student_id, course_id` in an enrollment table). The pair of foreign keys *is* the key.
- **Weak-entity tables** — a child that can't exist without its parent (an order line without its order), keyed by the parent's key plus a local discriminator.
- **Fact tables** — the grain of a fact is often defined by the combination of its dimension foreign keys; that combination acts as a composite (logical) key for the row.

### The catch — and why warehouses avoid them as PKs

Composite keys are correct, but they're **wide** and **awkward to join on**: every table that references the row must carry *all* the columns, every join must match on *all* of them, and indexes grow. In warehouse dimensions this friction is why we introduce a single **surrogate key** — one narrow integer that stands in for the whole composite — and keep the natural composite as attributes. That surrogate story runs through the last two sections of this module.

A composite key remains the honest way to express "these columns together identify the row." It ties directly to the next idea: several of those columns are usually **foreign keys** pointing at other tables — and foreign keys are what enforce that the links actually hold.
