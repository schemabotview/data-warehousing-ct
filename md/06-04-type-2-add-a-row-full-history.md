## Type 2 — add a row (full history)

**Type 2** is the workhorse of historical tracking, and the **most popular** SCD type. When an attribute changes, you **don't touch the old row** — you **expire it and insert a new one**. The dimension accumulates a full timeline of versions.

### Expire the old, insert the new

Ana moves from Madrid to Barcelona. Instead of overwriting, `DIM_CUSTOMER` ends up with **two rows**:

| customer_key | customer_id | city | effective_date | expiry_date | is_current |
|--------------|-------------|------|----------------|-------------|------------|
| 1101 | C-4471 | Madrid | 2021-06-01 | 2026-03-31 | N |
| 1108 | C-4471 | Barcelona | 2026-04-01 | 9999-12-31 | Y |

Both rows are the *same customer* — same **natural key** `C-4471` — but each version gets its **own surrogate key** (`1101`, then `1108`). The old row is stamped closed; the new row is open and marked current.

### This is where the surrogate key pays off

Because the fact joins on the **surrogate**, history stays correct automatically:

- Sales Ana made before April 2026 point at `customer_key = 1101` → **Madrid**.
- Sales after point at `customer_key = 1108` → **Barcelona**.

So "sales by region" reports each sale under the region that was true **when it happened** — the world **as it was** (as-was reporting). Nothing is rewritten; the past is preserved exactly. This is impossible with a natural-key join, which is precisely why warehouses insist on surrogates (module 04).

### The cost

Type 2 **grows the dimension** — a new row per tracked change — and needs the control columns (`effective_date`, `expiry_date`, `is_current`) plus ETL that knows the *expire-and-insert* dance. That machinery is the next section, and loading it is section 10.

> Type 2 keeps full history by versioning rows: expire the old, insert a new one with a fresh surrogate key. Every fact stays linked to the version true at its time — the gold standard when history matters.
