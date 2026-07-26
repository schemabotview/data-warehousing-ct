## Factless fact tables — events & coverage

Every fact table so far has held measures. The last type has **none** — a **factless fact table** is all foreign keys and no numeric measures. It records that an **event or a relationship happened**, and you get your metric by **counting the rows**.

### The measure is COUNT(*)

It sounds like a contradiction — a fact with no facts — but it's common and useful. The row's *existence* is the fact. Student attendance is the textbook case: one row per student per class per day means "was present." How many attended? `COUNT(*)`.

| student_key | class_key | date_key |
|-------------|-----------|----------|
| S27 | C4 | 01-Feb |
| S31 | C4 | 01-Feb |

There are no amounts to sum — just keys — and the count of rows is the answer.

### Two flavours

**1 · Event tracking** — records that something occurred: an attendance, a login, a customer-service contact, a product movement. `COUNT(*)`, grouped by dimension, gives the metric.

**2 · Coverage** — records what was **possible**, so you can analyse **what didn't happen**. Which products were **on promotion** on which day — whether or not they sold:

| product_key | promotion_key | date_key |
|-------------|---------------|----------|
| 204 | DIWALI | 01-Nov |

On its own that's a coverage fact. Its power comes from **comparison**: join it against FACT_SALES and find the promoted products that **never sold** — the promotions that missed. A sales fact can only tell you what happened; a coverage fact lets you ask about the **negative space**.

### Where it earns its place

- **Counting events** — attendance, enrolments, interactions, clicks.
- **Analysing absence** — no-shows, unsold promotions, gaps in coverage.
- **Audit & compliance** — proof that a required event did (or did not) occur.

> A factless fact is keys with no measures: its metric is `COUNT(*)`, and its superpower is letting you analyse **what didn't happen**.
