## Type 3 — add a column (limited history)

**Type 3** keeps history not by adding *rows* but by adding a **column**. Alongside the current value, the row carries a **"previous" column** — so you keep the current value and **one** prior value, in the same single row.

### Current and previous, side by side

Ana moves from Madrid to Barcelona. A Type 3 `DIM_CUSTOMER` keeps her **one** row and records both cities:

| customer_key | customer_id | name | current_city | previous_city |
|--------------|-------------|------|--------------|---------------|
| 1101 | C-4471 | Ana Ruiz | Barcelona | Madrid |

No new row, no new surrogate key — just a `previous_city` column filled in. Only the **latest change** survives: when the value changes, the old `current_city` slides into `previous_city`, and anything that *was* in `previous_city` is overwritten and lost.

### The narrow, specific use

Type 3 is rare, but it fits one shape of problem well: when you want to report facts under **both an old and a new grouping at once**. The classic case is a **re-org or re-alignment** — sales regions are redrawn, and for a transition period every fact should be viewable under **both** the old region and the new region.

Add `current_region` and `previous_region`, and a report can group by either — comparing the business under the old and new structure side by side, without the full row-versioning of Type 2.

### Where it falls short

- **Only one step of history** — a *second* move (Barcelona → Valencia) pushes Madrid out; you keep Valencia + Barcelona and lose Madrid entirely.
- **No dates** — you know the *previous* value but not *when* it changed, and can't reconstruct which value applied to a specific fact.

So Type 3 is a **limited-history** tool, not a substitute for Type 2. When you need the *full* timeline, or the *dates* of change, use Type 2; when you need only "current vs. prior" for dual-grouping, Type 3 is the lean fit.

> Type 3 adds a previous-value column: current + one prior value, no extra rows. Perfect for old-vs-new dual reporting during a re-org — but it remembers only the last change, and no dates.
