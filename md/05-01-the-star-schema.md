## The star schema

Modules 03 and 04 built the two pieces — the fact and the dimension. This module arranges them into the signature shape of the data warehouse: the **star schema**.

A star schema is **one central fact table surrounded by dimension tables**, each joined **directly** to the fact. Draw it and you see why it's named a star — the fact sits at the centre, and the dimensions radiate out like points:

```
        DIM_DATE      DIM_CUSTOMER
              \          /
    DIM_CHANNEL — FACT_SALES — DIM_PRODUCT
              /          \
        DIM_PROMOTION   (…)
```

### Why this shape

Recall module 02: OLTP systems **normalize** — they shatter data into many small tables to make writes safe. Analytics wants the **opposite**. A star deliberately keeps only two layers — a fact, and a single ring of **flat, denormalized dimensions** — so that:

- **Queries are fast** — every dimension is **one join** from the fact; there are no chains of joins to walk.
- **The model is simple** — anyone can look at a star and understand it, and BI tools generate the SQL automatically.
- **The pattern is uniform** — join the fact to the dimensions you need, filter and group by their attributes, aggregate the measures. Every question, the same shape.

### The trade it accepts

Flat dimensions repeat data — the same `region` on every customer row, the same `product_line` on every product row. That **redundancy costs storage**, and in an OLTP system it would risk update anomalies. The warehouse accepts it gladly: dimensions are loaded by controlled ETL, not by constant user writes, and storage is cheap next to the value of fast, simple analysis.

> The star is the default warehouse schema: a fact at the centre, flat dimensions one hop away. The rest of this module dissects it, contrasts it with the snowflake, scales it to a galaxy, and walks a real physical build.
