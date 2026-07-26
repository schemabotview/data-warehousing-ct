## Normalization — why we split tables

Imagine one wide table that records every order — and, on each order row, repeats the customer's name and city:

| order | customer | city |
|-------|----------|------|
| 1 | Ann | Delhi |
| 2 | Ann | Delhi |
| 3 | Ann | Delhi |

Ann's city is written three times. That single design choice creates three kinds of trouble:

- **Update anomaly** — Ann moves to Mumbai. Now you must find and change *every* row that mentions her. Miss one, and the table says she lives in two cities at once — the data contradicts itself.
- **Insertion anomaly** — you can't record a new customer until they place an order, because customer facts only exist on order rows.
- **Deletion anomaly** — delete Ann's last order and you erase the only record that she ever existed.

The root cause is **redundancy**: the same fact stored in more than one place. When a fact lives in many places, the copies can drift apart, and the table can no longer be trusted.

### The fix — one fact, one place

**Normalization** is the process of organizing tables to **reduce redundancy and improve data integrity**, by **splitting** data into related tables linked by keys. Each fact gets stored exactly once, in the table it belongs to:

- an **orders** table that records which customer placed each order, and
- a **customers** table that records each customer's city — *once*.

Now Ann's city lives in a single row. Change it there and every order sees the new value automatically. The anomalies disappear.

### Where it fits

Normalization is the natural design for **OLTP / transactional** systems — banking, ordering, booking — where writes are constant and correctness is everything. Splitting means each update touches one small row, safely.

The trade-off is that answering a question now requires **joining** the tables back together, which costs read performance. That tension — safe writes versus fast reads — is the whole story of this module: we normalize to protect data, then, for the warehouse, deliberately *denormalize* to make analysis fast. The next sections make the rules precise (the normal forms), walk a worked split to 3NF, and then turn the dial back the other way.
