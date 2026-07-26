## Additive, semi-additive & non-additive measures

The whole point of a measure is that you **add it up** across dimensions. But not every number behaves the same way under a `SUM`, and knowing which is which is what keeps a report from lying. Measures fall into three classes.

### Additive — sums across every dimension

An **additive** measure can be summed across **all** the fact's dimensions and still make sense. `line_total` is the model citizen: total sales for a day, a product, a region, a customer — every one is just a `SUM(line_total)` with a different `GROUP BY`. `quantity`, `discount_amount`, `tax_amount` are additive too. Additive measures are what fact tables exist for, and you want as many as you can get.

### Semi-additive — sums across some dimensions, not time

A **semi-additive** measure can be added across some dimensions but **not across time**. The classic case is a **balance** or a **snapshot level** — `on_hand_quantity` in the inventory snapshot. Add today's stock across all warehouses: fine, that's total stock on hand. Add the *same* warehouse's stock across every day of the month: nonsense — you've counted the same units thirty times. Across time you **average** (or take the last value), never sum. Account balances, headcount, temperature all behave this way.

### Non-additive — never summed

A **non-additive** measure cannot be summed across **any** dimension — usually because it is a **ratio or a rate**. `unit_price`, a profit **margin %**, a conversion rate: adding two prices together is meaningless. So you don't store the ratio pre-computed and sum it; you store its **additive components** — the numerator and denominator — and compute the ratio *after* aggregating. Keep `discount_amount` and `line_total` on the fact, and derive discount % as `SUM(discount_amount) / SUM(line_total)` at query time.

### The rule of thumb

| Class | Sum across… | Example |
|-------|-------------|---------|
| Additive | all dimensions | `line_total`, `quantity` |
| Semi-additive | all but time | `on_hand_quantity`, balance |
| Non-additive | none (ratios) | `unit_price`, margin % |

> Design for additivity: keep raw amounts on the fact and compute ratios last. A measure you can't add is a measure you can barely report on.
