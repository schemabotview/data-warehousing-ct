## Degenerate dimensions revisited

We met the **degenerate dimension** from the fact side in module 03. Seen from the *dimension* side, it completes the picture of the special dimension types — the one type that has **no table at all**.

### The same idea, from the dimension side

A degenerate dimension is a **dimension key that lives in the fact table with no dimension table of its own** — because there is nothing left to describe it. In the Jabra `FACT_SALES`, `order_id` is the classic case: it's a genuine dimension (you group and filter by it), but every attribute an "order dimension" might hold — the customer, the date, the channel — has already been pulled out into its **own** conformed dimension. What remains is a bare identifier. Give a bare identifier its own one-column table and you've built an empty, join-for-nothing table. So it **stays on the fact**.

### Still a real, useful dimension

Dimension-less doesn't mean useless — it earns its keep:

- **Groups a transaction's lines** — `GROUP BY order_id` reassembles the basket: items per order, order total, market-basket analysis.
- **Audit trail** — the operational id ties a fact row back to the source system.
- **An analytic handle** — count distinct orders, average lines per order.

### Placing it among the four special types

| Type | Table? | Holds |
|------|--------|-------|
| Conformed | shared table | rich attributes, reused across facts |
| Role-playing | one table, many joins | same dim in several roles |
| Junk | one small table | bundled low-cardinality flags |
| **Degenerate** | **no table** | **just an id, on the fact** |

> A degenerate dimension is the limiting case: a dimension whose every attribute has already become another dimension, leaving only the key — so it lives on the fact, table-less.
