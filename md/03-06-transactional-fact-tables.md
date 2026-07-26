## Transactional fact tables

Fact tables come in a few recognisable shapes, distinguished by **when and how rows are recorded**. The first — and by far the most common — is the **transactional** fact table.

### One row per event

A transactional fact records **one row per transaction, at the moment it happens**. FACT_SALES is exactly this: one row per order line, written when the line is sold. One ATM withdrawal, one website click, one phone call, one payment — each is a row. This is the **finest, most atomic grain** there is, and it's why transactional facts are the workhorse of the warehouse.

One row per sale, insert-only:

| sale_id | date | product | quantity | amount |
|---------|------|---------|----------|--------|
| 1 | 01-Feb | Headset | 2 | 240 |
| 2 | 01-Feb | Earbuds | 1 | 120 |

### Insert-only, and 1:1 with the source

Transactional rows are **inserted, never updated** — the event happened, and its record is immutable. And they map **one-to-one** with source transactions. If 240 orders land across Tuesday to Thursday in the source system, the warehouse fact gains exactly 240 rows:

| Day | Rows |
|-----|------|
| Tuesday | 80 |
| Wednesday | 70 |
| Thursday | 90 |
| **Total** | **240** |

### Mostly additive, endlessly rich

Because the grain is atomic and the measures are usually **additive** (`quantity`, `line_total`), a transactional fact rolls up to *any* summary the business asks for — by day, by product, by region — while still holding every individual event for the questions no one has asked yet. This flexibility is the reason you **default** to a transactional fact and only add the other types when a specific need appears.

> Transactional = one immutable row per event, at the lowest grain. It's the default fact table; the other types exist to solve the problems it can't.
