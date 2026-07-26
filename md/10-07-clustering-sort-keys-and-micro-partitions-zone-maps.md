## Clustering, sort keys & micro-partitions / zone maps

Columnar storage means you read only the columns you need. **Data skipping** goes further — it lets you avoid reading most of the *rows* too, **without an index**. This is how columnar MPP replaces B-trees.

### Zone maps — skip blocks that can't match

Warehouses split each column into **blocks** and store lightweight **metadata per block** — most importantly the **min and max** value. That metadata is a **zone map**. When a query filters, the engine checks each block's min/max and **reads only blocks whose range could contain a match**, skipping the rest:

```
WHERE order_date = '2026-04-12'
  block A  min 2020-01 max 2021-12   → skip
  block B  min 2026-01 max 2026-06   → read
  block C  min 2027-01 max 2027-12   → skip
```

- **Micro-partitions** (Snowflake) — data is auto-split into small immutable micro-partitions, each carrying these per-column stats.
- **Zone maps / block-range** (Redshift, BigQuery) — the same min/max-per-block idea.

### Clustering & sort keys — make skipping work

Zone maps only help if related values sit **together** in few blocks. If dates are scattered, every block's min/max spans everything and nothing can be skipped. So you **physically order** the data by the columns you filter on:

- **Sort key** (Redshift `SORTKEY`) — store the table sorted by, say, `order_date`.
- **Clustering** (Snowflake auto-clustering, BigQuery `CLUSTER BY`) — keep data grouped by chosen columns.

Cluster on `order_date` and all of April 2026 sits in a handful of micro-partitions with tight min/max → a date filter reads just those.

### The mental swap

Old world: *add an index* on filter columns. New world: **cluster/sort on the columns you filter by**, and let zone maps prune. Same aim — read less — different mechanism, one that suits columnar MPP.

> Zone maps store per-block min/max so the engine skips blocks that can't match; clustering and sort keys physically order data so those blocks are tight. Together they're columnar MPP's answer to the index — data skipping, not B-trees.
