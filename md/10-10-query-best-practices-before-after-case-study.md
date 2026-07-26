## Query best practices — before → after case study

The platform does a lot automatically — but a badly written query defeats all of it. A few habits let the machinery (columnar, pruning, zone maps, caching) actually work, and one before→after makes the difference concrete.

### The habits that matter

- **Name columns, never `SELECT *`** — columnar means you pay per column read; ask for the two you need, not all twenty.
- **Filter on partition / cluster keys** — especially dates; this is what triggers pruning and zone-map skipping.
- **Don't wrap a filter column in a function** — `WHERE order_date >= '2026-01-01'` prunes; `WHERE YEAR(order_date) = 2026` **defeats** pruning (the engine can't use the zone maps).
- **Filter early, aggregate late; join on keys** — shrink the row set before the expensive work.
- **`UNION ALL` over `UNION`** when you don't need dedup — skips a sort.
- **Let the platform help** — clustering, materialized views, result cache.

### Before → after

**✗ Slow**
```sql
SELECT * FROM fact_sales
WHERE YEAR(order_date) = 2026;
```
Reads **every column**, and the function on `order_date` **blocks partition pruning** → a full-table scan.

**✓ Fast**
```sql
SELECT product_key, line_total FROM fact_sales
WHERE order_date BETWEEN '2026-01-01' AND '2026-12-31';
```
Reads **two columns**, and the plain range lets the engine **prune to 2026's partitions**. Same result, a **fraction of the data scanned** — faster *and* cheaper (cloud bills by bytes scanned).

### Closing the course

That's the whole arc: **model** a clean star (03–08), **load** it reliably (09), **run** it on cloud MPP — columnar, pruned, cached (10) — and **write queries** that let all that machinery do its job.

> Write queries that cooperate with the platform: name columns, filter on partition/cluster keys, keep filters function-free so pruning works. Same answer, a fraction of the scan — the payoff of everything this course built.
