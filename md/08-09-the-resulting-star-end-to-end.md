## The resulting star — end to end

Put the pieces together and the design is complete: `FACT_SALES` at the centre, five dimensions around it — a **star**, derived entirely by running the four steps on one bill.

```
        DIM_DATE        DIM_CUSTOMER
              \            /
   DIM_CHANNEL — FACT_SALES — DIM_PRODUCT
              /            \
        DIM_PROMOTION   (order_id, order_status = degenerate)
```

### Trace the whole design

Every part of the star traces to a step:

- **Process** (step 1: sales order) → *the fact exists at all.*
- **Grain** (step 2: one order line) → *what a fact row means.*
- **Dimensions** (step 3: date, customer, product, channel, promotion) → *the points of the star.*
- **Facts** (step 4: quantity … line_total) → *the measures at the centre.*

Four questions in, a working star out.

### Prove it answers the business

The test of a model is whether it answers the questions that motivated it. Watch the star join do exactly that:

```sql
SELECT   d.month_name, p.product_line, c.region, SUM(f.line_total)
FROM     fact_sales f
JOIN     dim_date     d ON f.order_date_key = d.date_key
JOIN     dim_product  p ON f.product_key    = p.product_key
JOIN     dim_customer c ON f.customer_key   = c.customer_key
GROUP BY d.month_name, p.product_line, c.region;
```

"Revenue by month, by product line, by region" — one fact, three dimension joins, one aggregate. The design *works*: measures from the centre, slices from the points, every question the same shape.

### The four steps, mapped to the star

Process → the star's *subject*. Grain → its *rows*. Dimensions → its *points*. Facts → its *centre*. That's the whole method, and the whole star.

> Run the four steps and a validated star falls out: fact at the centre, dimensions as points, answering "measure by attribute" queries directly. Design proven by the query it was built to serve.
