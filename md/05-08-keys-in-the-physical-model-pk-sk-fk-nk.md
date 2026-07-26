## Keys in the physical model — PK, SK, FK, NK

Read any physical star diagram and you'll meet four key abbreviations tagged on columns: **PK, SK, FK, NK**. They're not four independent things — they're the roles keys play once the star is built. Getting them straight is how you read a model at a glance.

### The four roles

- **SK — surrogate key.** A warehouse-generated integer, meaningless to the business, minted for each dimension row. `customer_key = 1101`.
- **PK — primary key.** The column that uniquely identifies a row. In a **dimension, the PK *is* the surrogate key** — so you'll often see the tag written `PK|SK`.
- **FK — foreign key.** A column on the **fact** that references a dimension's PK. `FACT_SALES.customer_key` is an FK.
- **NK — natural (business) key.** The source system's own identifier, **kept as an attribute** on the dimension for traceability — never the join key. `customer_id = C-4471`.

### How they connect the star

```
FACT_SALES.customer_key   (FK)  ──►  DIM_CUSTOMER.customer_key  (PK|SK)
                                      DIM_CUSTOMER.customer_id   (NK) → source
```

The **FK on the fact** points at the **PK/SK of the dimension** — that single relationship, repeated per dimension, *is* the star. The **NK** rides along inside the dimension so you can always trace a warehouse row back to the operational system, but no fact ever joins on it.

### Why split SK from NK

This is modules 02 and 04 made physical. The join runs on the **surrogate** (fast integer, source-independent, and — crucially — able to carry SCD history, since two versions of one customer share a `customer_id` but get different `customer_key`s). The **natural** key is retained purely as an attribute — the audit trail home.

> Four tags, one story: the fact's **FK** joins to the dimension's **PK**, which is its **SK**; the **NK** sits beside it for traceability. Join on surrogates, keep naturals as attributes.
