## Why denormalized dimensions win for BI

The star's defining choice is that its dimensions are **denormalized** — flat. In module 02 we normalized hard to protect writes; here we deliberately reverse it. Why is flattening the right call for a warehouse?

### What "denormalized" means here

A normalized customer would split across tables: customer → city → province → region → country, each a lookup joined by key. A **denormalized** `DIM_CUSTOMER` collapses all of it into **one wide table** — `city`, `province`, `region`, `country` are just columns, repeated on every row that shares them.

### The four wins

- **Fewer joins → faster queries.** Join cost dominates analytic queries. Flat dimensions mean one join per dimension instead of a chain; the engine touches far fewer tables. This is the big one.
- **Simpler SQL.** "Sales by region" is *one* join to `DIM_CUSTOMER`, not four hops through a normalized geography tree. Analysts write it easily; BI tools generate it reliably.
- **Everything in one place.** All of a customer's attributes sit in one table — easy to browse, filter, and expose in a reporting tool. No hunting across lookup tables.
- **Attributes are discoverable.** One table *is* the list of ways you can slice — the dimension documents itself.

### Why the redundancy is safe

Normalization exists to prevent **update anomalies** from redundancy. In a warehouse those barely bite, because dimensions are **load-once, read-many**: ETL writes them in a controlled batch, and the same job updates every affected row consistently. There are no ad-hoc user updates to leave copies out of sync. So you get redundancy's *upside* — no joins — without paying its usual price.

The cost is **storage**, and it is cheap — trivial next to the query-speed and simplicity you gain.

> Normalize to protect writes (OLTP); denormalize to accelerate reads (the warehouse). Flat dimensions trade cheap storage for fewer joins, simpler SQL, and self-documenting slices — exactly what BI needs.
