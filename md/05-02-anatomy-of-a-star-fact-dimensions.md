## Anatomy of a star — fact + dimensions

Zoom into a star and it has exactly two kinds of part, wired together one way. Understanding those parts is understanding the whole schema.

### The centre — the fact

At the middle sits the **fact table**, `FACT_SALES`. It holds the **measures** (`quantity`, `line_total`) and a **foreign key to each dimension** (`customer_key`, `product_key`, `order_date_key`, …). Its grain — one order line — fixes what a row means. It is **long and narrow**: few columns, but millions of rows.

### The points — the dimensions

Around it sit the **dimension tables**, one per point. Each holds **descriptive attributes** (`name`, `category`, `region`) and a **surrogate primary key**. Each is **wide and shallow**: many columns, relatively few rows. `DIM_CUSTOMER`, `DIM_PRODUCT`, `DIM_DATE`, `DIM_CHANNEL`, `DIM_PROMOTION`.

### The wiring — one join per point

Every point connects the same way: a **foreign key on the fact references the surrogate primary key of the dimension**.

```
FACT_SALES.customer_key  ──►  DIM_CUSTOMER.customer_key  (PK)
FACT_SALES.product_key   ──►  DIM_PRODUCT.product_key    (PK)
```

The relationship is **one-to-many**: one customer, many sales rows; one product, many sales rows. Count the tables and you get the star's defining economy — **1 fact + N dimensions = N + 1 tables, and at most N joins** to answer any question. There is never a join *between* dimensions; they meet only *through* the fact.

### The query it's built for

That structure makes every analytic query the **star join**: fact in the `FROM`, dimensions `JOIN`ed on the keys, attributes in `WHERE`/`GROUP BY`, measures in the `SELECT` aggregate. Measures from the centre, labels from the points.

> A star is one fact (deep, narrow, measures + FKs) ringed by dimensions (wide, shallow, attributes + surrogate PK), each one join away — and every query joins the centre to the points it needs.
