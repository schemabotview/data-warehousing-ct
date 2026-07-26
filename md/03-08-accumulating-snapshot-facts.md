## Accumulating snapshot facts

The first two fact types never change a row once it's written. The **accumulating snapshot** is the exception — it's built for a **process with a defined lifecycle**, and its rows are **updated in place** as an item moves through the milestones.

### One row per item, updated through its journey

An accumulating snapshot has **one row per tracked item** — an order, a claim, a loan application — and a **column for each milestone date** in the pipeline. The row is born when the process starts, with the later dates NULL, and each milestone **fills in its date** as it's reached.

Jabra's order fulfilment is the natural example — order → packed → shipped → delivered:

| order_id | order_dt | packed_dt | shipped_dt | delivered_dt |
|----------|----------|-----------|------------|--------------|
| O001 | 10-Jan | 11-Jan | 12-Jan | 14-Jan |
| O002 | 13-Jan | 13-Jan | NULL | NULL |

O001 has completed the journey; O002 is packed but not yet shipped — its later dates are still open, waiting to be updated.

### Measures are the lags between milestones

The point of the type is **elapsed time**. Between the milestone dates you compute **lag measures** — days to pack, days to ship, days to deliver — and watch them as SLA metrics. `days_to_deliver` in the Jabra `FACT_SHIPMENT` is exactly this kind of derived duration. The pipeline itself is short and fixed:

```
Order → Packed → Shipped → Delivered
```

### What makes it different

- **It updates** — the *only* fact type whose existing rows are revisited and rewritten (transactional and snapshot facts are insert-only).
- **Grain = one lifecycle**, not one event or one period.
- **Best for pipelines** with a **known, finite set of steps** — fulfilment, claims, admissions, hiring.

> Reach for an accumulating snapshot to measure a **process end-to-end** — one row per item, milestone dates filling in, lag measures between them.
