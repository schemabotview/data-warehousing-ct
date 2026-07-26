## Building the order fact

With the dimensions built, assemble the centre: **`FACT_SALES`**, at grain **one order line**. Its columns come straight from the four steps — foreign keys, measures, and degenerate dimensions.

### The three column groups

```
FACT_SALES  — grain: one order line
  sales_key                                 -- surrogate PK
  order_date_key, customer_key, product_key, -- FKs (step 3 dims)
  channel_key, promotion_key
  order_id, order_line_id, order_status      -- degenerate dims
  quantity, unit_price, discount_amount,     -- measures (step 4)
  tax_amount, line_total
```

- **Foreign keys** — one surrogate FK per dimension from step 3.
- **Measures** — the M fields from step 4; `line_total` is a **derived** measure (`quantity × unit_price − discount + tax`, module 05).
- **Degenerate dimensions** — `order_id`, `order_status`, riding on the fact.

### The surrogate-key lookup (the ETL heart)

The bill carries only **natural** keys (`C-4471`, `P-88`). The load must translate each to its **surrogate** before writing the fact row: look `C-4471` up in `DIM_CUSTOMER` → get `customer_key = 1101`; look `P-88` up in `DIM_PRODUCT` → `product_key = 204`. That **key-lookup** step is what wires a fact row to the *correct version* of each dimension (and it's the core of module 09's load).

### One bill → two fact rows

Grain discipline in action: the bill has **two line items**, so it produces **two** `FACT_SALES` rows — same `order_id` (a degenerate dimension grouping them), different `product_key`s and measures. The header values (customer, date, channel) repeat on both lines as their respective foreign keys.

> The order fact is FKs (per dimension) + measures (additive, with a derived `line_total`) + degenerate dimensions, at one row per order line — populated by looking each natural key up to its dimension's surrogate.
