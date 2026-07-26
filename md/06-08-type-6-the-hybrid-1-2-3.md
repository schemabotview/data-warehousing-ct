## Type 6 — the hybrid (1 + 2 + 3)

**Type 6** is the combination type — **1 + 2 + 3 = 6** (the number is literally the sum). It layers all three onto one dimension so you can report **both** "as it was" **and** "as it is" from the same table.

### All three at once

A Type 6 row has the **Type 2** structure (a new versioned row per change, with effective/expiry/current), and *also* carries an extra **current-value** column that behaves like **Type 3** but is kept up to date **Type 1**-style on every historical row:

| customer_key | customer_id | historical_region | current_region | effective | expiry | is_current |
|--------------|-------------|-------------------|----------------|-----------|--------|------------|
| 1101 | C-4471 | Centro (Madrid) | Cataluña | 2021-06-01 | 2026-03-31 | N |
| 1108 | C-4471 | Cataluña | Cataluña | 2026-04-01 | 9999-12-31 | Y |

- **`historical_region`** — frozen to what was true for that version (the **Type 2 / as-was** value).
- **`current_region`** — the entity's *latest* region, **overwritten on every row** for that customer whenever it changes (the **Type 1 + Type 3 / as-is** value).

### What it buys you

From one table, two questions, no re-modelling:

- **As-was** — "revenue by the region **at the time** of sale" → group by `historical_region`. Ana's old sales credit **Madrid's** region.
- **As-is** — "revenue by the customer's **current** region" → group by `current_region`. *All* of Ana's sales, old and new, credit **Cataluña**.

That flexibility — analyzing history under either the then-current or the now-current attribute — is Type 6's whole reason to exist.

### The cost

More columns and **more ETL work**: each change must insert the new Type 2 row *and* sweep the `current_*` column across **all** prior rows of that entity. It's the most capable and the most complex of the common types.

> Type 6 fuses 1 + 2 + 3: versioned rows for full history, plus an overwritten current column on every row — so one dimension answers both as-was and as-is, at the price of heavier ETL.
