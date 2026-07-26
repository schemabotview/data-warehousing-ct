## Loading dimensions before facts

There's a hard rule about the **order** of a load: **load the dimensions first, then the facts**. It follows directly from the surrogate-key lookup — and breaking it corrupts the star.

### Why the order is forced

A fact row needs a **surrogate FK for every dimension key** it carries. That surrogate **only exists** once the dimension row has been loaded and its key generated. So:

1. **Load / merge the dimensions** — insert new members, apply SCD changes (module 06). Every needed surrogate key now exists.
2. **Load the facts** — the lookups (section 07) all resolve, because the dimension rows are already there.

Load facts first and every lookup **misses** — rows get rejected, or worse, silently sent to the **"Unknown" member**, and your star loses its links. Dimensions before facts guarantees **referential integrity**: every fact FK points at a real dimension row.

### Early-arriving facts

Sometimes a fact shows up **before** its dimension data — an order for a customer the customer feed hasn't delivered yet (an *early-arriving fact*). You don't drop it. You:

- Insert an **inferred (placeholder) dimension member** — the natural key is known from the fact, the descriptive attributes are `NULL` / Unknown for now.
- Point the fact at that placeholder's surrogate key.
- **Backfill** the real attributes when the dimension feed catches up (an SCD/update on that row).

This keeps the fact **and** its FK valid while gracefully handling out-of-order arrival.

> Always load dimensions before facts, so every fact's surrogate-key lookup resolves and referential integrity holds. For early-arriving facts, insert an inferred placeholder member and backfill it later.
