## Hash keys & load metadata

Every table in the vault — hub, link, satellite — carries the same set of **metadata columns**. They're not decoration: they're the machinery that delivers the vault's two promises, **parallel loading** and **auditability**.

### The hash key (`_hk`)

The surrogate primary key of every hub and link is a **hash key** — an MD5/SHA hash of the **business key(s)**:

```
customer_hk    = hash(customer_id)
order_line_hk  = hash(order_hk + product_hk)
```

Why hash instead of a database sequence? Because a hash is **deterministic**: the same business key produces the same hash on **any** system, with **no lookup**. That single property is what lets hubs, links, and satellites load **independently and in parallel** — a link can compute its parent hash keys *itself*, without first querying the hubs to find a sequence number. It's what makes the vault scale on distributed / MPP platforms.

### The load metadata

- **`load_date`** — when the row entered the vault. Sequencing satellite rows by `load_date` *is* the history.
- **`record_source`** — where the row came from (`'web'`, `'pos'`, `'erp'`). Every single row carries its origin, so lineage is never lost.

Together, `load_date` + `record_source` on every row is the **audit trail**: for any value, you can say exactly **when** it arrived and **from which system**.

### `hash_diff` — cheap change detection

Satellites add one more: **`hash_diff`**, a hash of **all the descriptive attributes** in the row. On load, you compare the incoming `hash_diff` to the parent's latest satellite row:

- **Differs** → something changed → **insert** a new satellite version.
- **Matches** → nothing changed → **skip**.

It's the same hash trick from the SCD-2 merge (module 06), and it means you never diff columns one by one.

> The `_hk` hash key makes loads lookup-free and parallel; `load_date` + `record_source` stamp every row for audit; `hash_diff` detects change in one comparison. This metadata is how the vault delivers scale and auditability.
