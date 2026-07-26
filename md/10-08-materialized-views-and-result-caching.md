## Materialized views & result caching

The fastest query is the one you **don't recompute**. Two platform features avoid recomputation — **result caching** and **materialized views** — and they layer neatly.

### Result caching — reuse an identical query's answer

The platform **caches a query's result**; run the **same** query again and it returns **instantly from cache**, with no rescan. Dashboards that fire the same query all morning pay for it once. The cache is **invalidated when the underlying data changes**, so you never see a stale answer. (Snowflake result cache, BigQuery cached results — automatic, free.)

### Materialized views — precompute an aggregate

A **materialized view (MV)** is a **stored, precomputed query result** — typically an aggregate — that the platform **keeps up to date** as the base data changes. This is the **aggregate fact from module 03**, now maintained by the engine instead of hand-rolled ETL. An MV of *monthly sales by product line* answers that dashboard from a few thousand pre-rolled rows instead of scanning the whole fact.

The clever part: the optimizer can **automatically rewrite** a query written against the **base** `FACT_SALES` to use the MV when it covers the request — you get the speed-up without changing the query.

### Same trade-off as aggregates

An MV costs **storage + refresh**, in exchange for **query speed** — exactly the aggregate-fact bargain (modules 03/09). Build MVs for **known, repeated, expensive aggregations**; keep the atomic fact as the source of truth beneath them.

### Layered

- **MV** cuts the compute (roll-up already done).
- **Result cache** cuts it to **zero** on an exact repeat.

Together they make dashboards feel instant.

> Result caching returns an identical query from cache (auto-invalidated on data change); materialized views store an engine-maintained aggregate the optimizer can auto-substitute. Both trade storage/refresh for speed — the aggregate-fact bargain, automated.
