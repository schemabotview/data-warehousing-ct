## Aggregate (summary) fact tables

An atomic transactional fact is flexible but **big** — hundreds of millions of order lines. When a dashboard asks the same coarse question again and again ("monthly sales by product line"), re-summing all those rows every time is wasteful. An **aggregate fact** stores the answer **pre-computed**.

### Pre-summarised, at a coarser grain

An aggregate fact is a fact table **rolled up** from the atomic one to a **coarser grain**, built and maintained by ETL. From per-order-line FACT_SALES you might derive monthly totals per product line:

| month | product_line | total_qty | total_sales |
|-------|--------------|-----------|-------------|
| 2026-01 | Headsets | 12,400 | 345,000 |
| 2026-01 | Earbuds | 9,800 | 210,000 |

The measures are the **same additive measures**, `SUM`-med up a level. It's a genuine fact table — keys plus measures — just at reduced detail. (A periodic snapshot is often a form of aggregate.)

### Why build one

- **Speed** — a dashboard reads a few thousand pre-rolled rows instead of scanning hundreds of millions. Often a 100× win on a hot query.
- **Cost** — fewer rows scanned is cheaper on cloud / MPP platforms.

### The trade-off — it's a derived copy

An aggregate carries a duty:

- **It must stay consistent** — rebuilt or incrementally refreshed by ETL whenever the base fact changes; a stale aggregate lies.
- **It only answers at its grain** — a monthly aggregate can't answer a daily question. Keep the atomic fact as the **source of truth** and treat aggregates as a **performance layer** on top, never a replacement.

On modern cloud/MPP platforms the engine can maintain these for you as **materialized views** — same trade-off, less hand-rolled ETL (module 10).

> Aggregates trade storage and ETL effort for query speed. Build them for known, repeated, coarse queries — and always keep the atomic fact beneath them.
