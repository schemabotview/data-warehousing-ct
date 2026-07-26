## Foreign keys & referential integrity

Normalization scatters facts across many tables. A **foreign key** is what stitches them back together — and what guarantees the stitching is sound.

### What a foreign key is

A **foreign key (FK)** is a column (or set of columns) in one table that **references the primary key of another table**. It's the pointer that says "this row belongs with *that* row." In our split from earlier, `orders.cust_id` is a foreign key pointing at `customers.cust_id` — every order names the customer it belongs to.

```sql
CREATE TABLE DEPARTMENT (
  DEPT_ID   INT PRIMARY KEY,
  DEPT_NAME VARCHAR(28)
);

CREATE TABLE STUDENT (
  ID      INT PRIMARY KEY,
  NAME    VARCHAR(28),
  DEPT_ID INT,
  FOREIGN KEY (DEPT_ID) REFERENCES DEPARTMENT(DEPT_ID)
);
```

### Referential integrity — the rule it enforces

The foreign key isn't just documentation; the database **enforces** it. **Referential integrity** is the guarantee that every foreign-key value must **exist** in the parent table (or be NULL). Concretely:

- You **can't insert** a `STUDENT` with `DEPT_ID = 9` if no department 9 exists — no orphan rows.
- You **can't delete** a `DEPARTMENT` that still has students pointing at it (unless you cascade or set NULL) — no dangling references.

The result: the links in your normalized model can be *trusted*. A join never silently drops rows or points at nothing.

### In the warehouse

A star schema is one giant application of this idea: the **fact table's foreign keys** each reference a **dimension's primary (surrogate) key**. `fact_sales.customer_sk → dim_customer.customer_sk`, `product_sk → dim_product`, `date_sk → dim_date`, and so on. Those FK-to-PK links *are* the star's rays.

One nuance: warehouses often **don't physically enforce** FK constraints at load time — checking them on every insert would slow big batch loads. Instead the **ETL process guarantees integrity** (it resolves every key via lookups before loading, covered in Module 09), and the constraints may be declared but disabled, kept for documentation and to inform the optimizer. The *concept* is essential even where the *enforcement* is delegated to the pipeline.
