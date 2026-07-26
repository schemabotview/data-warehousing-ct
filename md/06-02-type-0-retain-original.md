## Type 0 — retain original

The simplest strategy is to **never change the value at all**. That's **Type 0** — *retain original*. The attribute is set when the row is first loaded, and every later update from the source is **ignored**.

### Write once, then freeze

Where Type 1 (next section) *overwrites* with each new value, Type 0 *refuses* it. The original stands forever:

| customer_key | customer_id | name | date_of_birth | original_signup_date |
|--------------|-------------|------|---------------|----------------------|
| 1101 | C-4471 | Ana Ruiz | 1990-03-12 | 2021-06-01 |

If a bad feed later sends a different `date_of_birth`, Type 0 keeps `1990-03-12`. The column is effectively **write-once**.

### When "original" is the point

Type 0 is right for attributes that are **immutable by nature** or whose **original value is the meaningful one**:

- **Truly fixed facts** — `date_of_birth`, a country of birth, a product's original launch date.
- **"Original" measures you want to preserve** — the customer's `original_signup_date`, the `original_credit_score` at account opening, the first-ever plan they joined on.

The intent is explicit: even if the source *can* change this field, the warehouse **chooses** to hold the original, because the analytical question is about the beginning, not the present.

### The trade-off

Type 0 keeps **no** current value and **no** history of change — just the first value, permanently. If the business later cares about the *current* value of that attribute, Type 0 is the wrong choice; you'd model it (or a companion column) as Type 1 or Type 2 instead.

> Type 0 is write-once: capture the original and never touch it. Use it for genuinely immutable facts and "as-originally-recorded" values — not for anything whose present value matters.
