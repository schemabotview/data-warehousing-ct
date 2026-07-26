## What a fact table is — measures & grain

Every question you ask a warehouse — "how much did we sell last quarter?", "which region is growing?" — is answered by adding up numbers. The table that holds those numbers is the **fact table**, and it sits at the dead centre of the schema.

A fact table stores the **measurements of a business process**. For Jabra Spain, the process is *selling*, and the fact is `FACT_SALES`. Each row is one **order line** — one product on one order — and it carries two very different kinds of column:

- **Foreign keys** — slim integer pointers to the dimensions that give the row its context: `order_date_key`, `customer_key`, `product_key`, `channel_key`, `promotion_key`. They answer *who, what, when, where*.
- **Measures** — the numeric facts you actually do arithmetic on: `quantity`, `unit_price`, `discount_amount`, `tax_amount`, `line_total`.

Keys on the left, measures on the right:

| order_date_key | customer_key | product_key | quantity | line_total |
|----------------|--------------|-------------|----------|-----------|
| 20260112 | 1101 | 204 | 2 | 1,200.00 |
| 20260112 | 1120 | 207 | 1 | 300.00 |

The keys are the *context*; the measures are the *numbers*. And almost every report is the same shape: pick some measures, group them by some dimension attributes, add them up — `SUM(line_total)` by `product_line`, by `month`, by `region`.

### What makes a fact table a fact table

- **Numeric & measurable** — measures are quantities you aggregate (sum, average, count).
- **Grain** — every row means the *same* thing, at one fixed level of detail. FACT_SALES is one order line — never mixed with order totals.
- **Keyed to dimensions** — a foreign key to every dimension that describes the event.
- **Long and narrow** — few columns, but *many* rows; this is the biggest table in the warehouse and the one that grows forever.

### Fact vs dimension in one line

Dimensions are the nouns and adjectives — customer, product, date; *which* and *what kind*. Facts are the verbs and the numbers — *what happened* and *how much*. Get those two apart and the whole model falls into place. And the single most important choice about a fact table is its **grain** — what one row means — which is exactly where we go next.
