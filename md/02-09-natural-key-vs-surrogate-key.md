## Natural key vs surrogate key

When you pick a primary key, you face a choice between two philosophies: use an identifier the **business already has**, or **invent** one. That choice is natural key versus surrogate key.

### Natural key — from the real world

A **natural key** is a column drawn from real business data that already uniquely identifies a record — an `email`, a `PAN`, an `ISBN`, an `SSN`, a product `SKU`. It has **business meaning** and exists out in the world. Its weakness: real-world identifiers **can change**, and they're often text — bigger and slower to index and join.

### Surrogate key — invented by the system

A **surrogate key** is an artificial identifier the system generates — typically an **auto-incrementing integer** like `customer_wid = 1001`. It has **no business meaning**, doesn't exist outside the database, and, crucially, **never changes**. It isn't derived from the row's data at all (in SQL Server, the `IDENTITY` property generates it without touching load performance).

### The problem a surrogate solves — a changing email

An online store keys its Customer dimension on the natural key `email`. Anita signs up as `anita@old.com` and places orders. Later she changes her email to `anita@new.com`.

✗ **Natural key only** — the key changed, so her orders look like two different people:

| order | customer (email) |
|-------|------------------|
| #1 | anita@old.com |
| #2 | anita@new.com |

✓ **Surrogate key** — identity stays stable while the email is just an attribute:

| cust_wid | email |
|----------|-------|
| 1001 | anita@old.com |
| 1001 | anita@new.com |

Both orders still map to `cust_wid 1001` — **one customer**. The stable surrogate is also what makes **SCD Type 2** possible: you can keep several historical rows for the same real customer, each with its own surrogate, all tracing back to one business identity.

### Side by side

| Aspect | Natural | Surrogate |
|--------|---------|-----------|
| Source | Business data | System-generated |
| Meaning | Yes | None |
| Can change? | Yes (risky) | Never |
| Size / join speed | Often text, slower | Small int, fast |
| Supports SCD-2 | No | Yes |
| Multi-source integrate | Conflicts | Clean |

**Best practice:** in warehouse dimensions, use a **surrogate key as the primary key**, and **keep the natural key as an attribute** so you can still trace a row back to its source system. *Why* the warehouse leans so hard on surrogates — beyond this one example — is the closing section.
