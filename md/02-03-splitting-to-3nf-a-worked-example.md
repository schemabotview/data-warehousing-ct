## Splitting to 3NF — a worked example

Rules are easier to trust once you've applied them. Let's take one redundant table all the way to third normal form.

### Where we start — one flat table

An order-tracking sheet where each row is an order, but customer details ride along on every row:

| order | cust | cust_city |
|-------|------|-----------|
| 1 | Ann | Delhi |
| 2 | Ann | Delhi |
| 3 | Bob | Pune |

`cust_city` is repeated for every order the customer places. This is the redundancy that breeds update, insertion, and deletion anomalies.

### Spot the transitive dependency

The primary key is `order`. Ask what each column really depends on:

- `cust` depends on `order` — fine, each order has one customer.
- `cust_city` depends on `cust`, and `cust` depends on `order`. So `cust_city` depends on the key only **through** `cust` — a **transitive dependency**. That is exactly what 3NF forbids.

### Split it out

Move the customer facts into their own table, keyed by the customer, and leave a link behind in orders:

✓ **orders**

| order | cust_id |
|-------|---------|
| 1 | C1 |
| 2 | C1 |
| 3 | C2 |

✓ **customers**

| cust_id | name | city |
|---------|------|------|
| C1 | Ann | Delhi |
| C2 | Bob | Pune |

Each order now stores a **foreign key** (`cust_id`) pointing at the one customer row. The city is written **once** per customer.

### What we gained

- **No update anomaly** — change Ann's city in a single row; every order sees it.
- **No insertion anomaly** — add a customer who hasn't ordered yet.
- **No deletion anomaly** — deleting an order no longer erases the customer.
- To read "orders with their city," you **join** the two tables on `cust_id`.

That last point is the catch: the normalized model is safe to write but needs joins to read. For a warehouse serving heavy analytical queries, those joins become the cost we push back against — which is where **denormalization** comes in next.
