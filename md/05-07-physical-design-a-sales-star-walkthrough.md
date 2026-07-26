## Physical design — a Sales star walkthrough

Enough theory — let's read a real one. Here is the Jabra **Sales star**, table by table, as it would actually be built.

### FACT_SALES — the centre

Grain: **one order line**. Its columns fall into three groups:

```
FACT_SALES
  sales_key                              -- surrogate PK
  order_date_key, customer_key,          -- FKs → dimensions
  product_key, channel_key, promotion_key
  order_id, order_line_id, order_status  -- degenerate dimensions
  quantity, unit_price, discount_amount, -- measures
  tax_amount, line_total
```

Foreign keys out to every dimension, a few degenerate dimensions with no table, and the additive measures — that's the whole fact.

### The dimension points

- **`DIM_DATE`** — one row per day, `YYYYMMDD` key, calendar + fiscal attributes. The universal, role-playing dimension.
- **`DIM_CUSTOMER`** *(SCD-2)* — `customer_key` (SK), `customer_id` (NK), name, segment, `city`/`province`/`region`/`country`, plus `effective_date` / `expiry_date` / `is_current` history columns.
- **`DIM_PRODUCT`** *(SCD-2)* — `product_key` (SK), `product_id` (NK), name, `product_line`, `category`, `brand`, `list_price`, plus SCD-2 columns.
- **`DIM_CHANNEL`** — `channel_key`, `channel_name` (Web, App, Amazon, Retail), `channel_type`.
- **`DIM_PROMOTION`** — `promotion_key`, name, `discount_type`, `discount_value`.

### The design choices worth naming

1. **Surrogate key on every table** — stable, source-independent, fast integer joins.
2. **Natural keys kept as attributes** (`customer_id`, `product_id`) — to trace a row back to the source.
3. **SCD-2 columns** on Customer and Product — they change over time and need history (module 06).
4. **Audit columns** — `insert_dt` / `update_dt` on the dimensions record when ETL loaded or changed each row.
5. **Degenerate dimensions** — `order_id`, `order_status` live on the fact, no table.

> A physical star is unglamorous and repeatable: a fact of FKs + degenerates + measures, ringed by surrogate-keyed dimensions that keep their natural keys, history columns where needed, and audit stamps throughout.
