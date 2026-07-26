## Conformed dimensions & the bus matrix

A warehouse has many fact tables — Jabra has sales, shipments, payments, inventory. If each one had its *own* customer table, its *own* date table, you could never compare across them: "revenue by region" and "shipments by region" would be using two different "regions". The fix is the **conformed dimension**.

### One dimension, shared, with the same meaning

A **conformed dimension** is a dimension that means **exactly the same thing across every fact that uses it** — physically the *same* table (or a consistent copy). In the Jabra galaxy, `DIM_DATE`, `DIM_CUSTOMER`, and `DIM_PRODUCT` are conformed — shared by `FACT_SALES`, `FACT_SHIPMENT`, and `FACT_PAYMENTS` alike:

```
FACT_SALES ────┐
FACT_SHIPMENT ─┼── DIM_CUSTOMER  (conformed)
FACT_PAYMENTS ─┘
```

Because every process rolls up by the *same* `region`, the *same* `product_line`, you can **line metrics up side by side** — revenue, units shipped, and cash collected, all "by region, by month" — and even **drill across** from one process to another. Conformed dimensions are the mechanism that turns separate data marts into one integrated warehouse.

### The bus matrix — the plan on one page

Kimball's **bus matrix** is the blueprint: **rows are business processes** (the facts), **columns are dimensions**, and a mark says "this process uses this dimension."

| Process | Date | Customer | Product | Warehouse | Carrier |
|---------|------|----------|---------|-----------|---------|
| Sales | ✓ | ✓ | ✓ | | |
| Shipment | ✓ | ✓ | ✓ | ✓ | ✓ |
| Payments | ✓ | ✓ | | | |

Read down a column and you see which dimensions are **shared** — those are the ones you must conform. Read across a row and you see a fact's grain in dimensions. The matrix is built up front, before any table, so the whole enterprise agrees on a single `DIM_CUSTOMER` instead of five incompatible ones.

> Conform the shared dimensions and separate marts snap together into one warehouse. The bus matrix is where you decide, on one page, which dimensions those are.
