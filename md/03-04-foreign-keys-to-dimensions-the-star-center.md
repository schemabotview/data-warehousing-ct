## Foreign keys to dimensions — the star center

Strip the measures off a fact row and what's left is a bundle of **foreign keys** — one pointer per dimension. Those keys are what make the fact the **centre of the star**: each key is a spoke reaching out to a dimension table, and the fact sits in the middle holding them together.

### One foreign key per dimension

FACT_SALES carries `order_date_key`, `customer_key`, `product_key`, `channel_key`, `promotion_key` — a slim integer for every dimension that describes a sale. Each references the **surrogate primary key** of its dimension: `customer_key` points at `DIM_CUSTOMER.customer_key`, `product_key` at `DIM_PRODUCT.product_key`, and so on. The fact stores only the key, never the descriptive text — the customer's name and city live *once* in the dimension, not repeated across millions of fact rows.

### How a query lights up the star

This is the shape of every BI query: **join** the fact to the dimensions on those keys, **filter and group** by dimension attributes, **aggregate** the measures.

```sql
SELECT   p.product_line, SUM(f.line_total)
FROM     fact_sales f
JOIN     dim_product  p ON f.product_key    = p.product_key
JOIN     dim_date     d ON f.order_date_key = d.date_key
WHERE    d.year = 2026
GROUP BY p.product_line;
```

The measures come from the centre; the labels and filters come from the points. That single star-join pattern answers almost every analytical question — which is exactly why the schema is shaped this way.

### Why surrogate integer keys

The foreign keys are deliberately **narrow integers**, not natural business codes:

- **Joins are fast** — integer key comparisons are the cheapest join there is, and the fact table is huge.
- **Storage is small** — a 4-byte key beats a repeated varchar on every one of millions of rows.
- **They absorb change** — because the fact points at a surrogate, a dimension can track history (SCD) without the fact ever changing.

> The fact's foreign keys *are* the star. Keep them surrogate, keep them not-null, and every dimension is one join away.
