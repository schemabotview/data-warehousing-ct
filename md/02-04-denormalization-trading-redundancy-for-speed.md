## Denormalization — trading redundancy for speed

Normalization removed redundancy to make writes safe. But we saw the price: every question now requires **joins**. For a warehouse, where a single report may scan millions of rows and join a dozen tables, that price is steep. So we deliberately turn the dial back.

**Denormalization** is the intentional **adding of redundancy** — merging tables, or duplicating columns — to improve **read performance** by reducing the number of joins. It is not sloppy design; it is a considered trade, made where fast queries matter more than compact storage.

### Why it's the right call for a warehouse

- **Fewer joins** — pre-joining related data into one wide table means the query engine reads one table instead of stitching several together.
- **Simpler queries** — analysts and BI tools hit a flat, readable structure.
- **Reads dominate** — a warehouse is loaded in controlled batches and queried constantly; optimizing for the read is optimizing for the common case.

This is precisely why a **dimension** table in a star schema is denormalized. Instead of `product → subcategory → category → department` spread across four normalized tables, all of it collapses into one flat `dim_product` row. One join from the fact reaches every level of the hierarchy.

### What you give up

The redundancy that normalization fought comes back, so denormalization carries its old risks — now *managed* rather than avoided:

- **More storage** — data is duplicated across rows.
- **Update cost / anomaly risk** — a changed value must be rewritten everywhere it was copied. In a warehouse this is acceptable because updates arrive through a **controlled ETL load**, not ad-hoc writes — the pipeline keeps the copies consistent.

### Normalize vs denormalize — the trade

| Aspect | Normalization | Denormalization |
|--------|---------------|-----------------|
| Redundancy | Minimized | Added on purpose |
| Joins | More | Fewer |
| Writes | Fast, safe | Slower / anomaly risk |
| Reads | Slower | Fast |
| Best for | **OLTP** transactional | **OLAP** / reporting |

The rule of thumb: **normalize for writes, denormalize for reads.** Operational systems in 3NF; warehouse dimensions flattened for speed. With that trade understood, the rest of the module turns to the keys that make both models work.
