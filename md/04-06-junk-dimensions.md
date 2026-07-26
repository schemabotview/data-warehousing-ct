## Junk dimensions

A transactional fact tends to collect a handful of **low-cardinality flags** — an order status, a payment flag, a gift-wrap indicator, a priority. Left on the fact, each is a separate column of tiny, repetitive codes, and they clutter the row. The tidy solution is a **junk dimension**.

### Bundle the flags into one small table

A **junk dimension** gathers several unrelated, low-cardinality flags and statuses into **one small dimension table**, and the fact carries a single `junk_key` in their place. Instead of four flag columns on millions of fact rows, the fact holds one key; the junk table holds the **distinct combinations** that actually occur:

| junk_key | order_status | payment | gift_wrap | priority |
|----------|--------------|---------|-----------|----------|
| 1 | Shipped | Paid | No | Standard |
| 2 | Shipped | Paid | Yes | Express |
| 3 | Returned | Refunded | No | Standard |

The "junk" isn't garbage — it's the useful-but-miscellaneous flags that don't each deserve their own dimension. Ten flags with a few values each might combine into only a few hundred real rows, because most theoretical combinations never happen (you only store the ones that do).

### Why bother

- **Keeps the fact narrow** — one key instead of many flag columns on a huge table.
- **Gives the flags a home** — they can be grouped, filtered, and labelled like any other dimension attribute.
- **Tidies the model** — a row of loose indicators becomes one clean dimension.

### Junk vs its cousins

- **Junk** — several flags → **one small table**, fact holds a key.
- **Degenerate** — an id with *no* attributes → stays **in the fact, no table** (next section).
- **Conformed** — a rich dimension **shared** across facts.

> When flags pile up on the fact, sweep them into a junk dimension: one key on the fact, one small table of the combinations that occur.
