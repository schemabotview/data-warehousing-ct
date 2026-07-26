## The normal forms — 1NF, 2NF, 3NF

Normalization proceeds in stages called **normal forms** — each one a rule a table must satisfy, each building on the one before. In practice, dimensional designers aim for **third normal form (3NF)**; the first three rules carry almost all the value.

First, two dependencies to name, because the rules are stated in terms of them:

- **Partial dependency** — a non-key column depends on only *part* of a composite (multi-column) key, not the whole key.
- **Transitive dependency** — a non-key column depends on *another non-key column*, rather than directly on the key.

### 1NF — atomic values

Every cell holds a **single value** — no lists, no repeating groups, no "phone1, phone2, phone3" columns. Each row is unique, identified by a primary key. First normal form makes the table a proper table: one value per cell, one row per real thing.

### 2NF — no partial dependency

The table is in 1NF **and** every non-key column depends on the **whole** primary key, not just part of it. This only bites when the key is **composite**. If `(order_id, product_id)` is the key but `product_name` depends on `product_id` alone, that's a partial dependency — split `product_name` out into a product table.

### 3NF — no transitive dependency

The table is in 2NF **and** no non-key column depends on another non-key column. If `order_id → customer_id → customer_city`, then `customer_city` depends on the key only *through* `customer_id` — a transitive dependency. Move `customer_city` to a customer table keyed by `customer_id`. In 3NF, every non-key column depends on **the key, the whole key, and nothing but the key**.

### BCNF — the stricter cousin

**Boyce-Codd Normal Form** tightens 3NF: every **determinant** (any column that determines another) must itself be a **candidate key**. It resolves rare edge cases with overlapping candidate keys. Most designs stop at 3NF; BCNF is worth knowing but seldom the deciding factor.

### The rules at a glance

| Form | Rule | Fixes |
|------|------|-------|
| **1NF** | Atomic cells, unique rows | Repeating groups / lists |
| **2NF** | No partial dependency | Column tied to part of a composite key |
| **3NF** | No transitive dependency | Column tied to another non-key column |
| **BCNF** | Every determinant is a candidate key | Overlapping candidate-key edge cases |

The mechanics of applying these — taking a redundant table apart step by step — are the subject of the next section.
