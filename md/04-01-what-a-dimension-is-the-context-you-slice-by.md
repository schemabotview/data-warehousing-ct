## What a dimension is — the context you slice by

A fact table holds the numbers. But a number on its own — `1,200` — means nothing. *Whose* sale? Of *what*? *When*? The tables that answer those questions are the **dimensions**, and they supply the **context you slice by**.

Look at any report and you'll see the pattern: "**sales _by_ product line**", "**revenue _by_ month, by region**". The measure comes from the fact; the **"by …"** is *always* a **dimension attribute**. Dimensions are how a business *thinks* about its numbers.

### What a dimension table looks like

A dimension is a table of **descriptive context** — the nouns and adjectives of the model. `DIM_PRODUCT` in the Jabra warehouse:

| product_key | product_id | name | category | product_line | brand |
|-------------|-----------|------|----------|--------------|-------|
| 204 | P-88 | Evolve2 65 | Headset | Headsets | Jabra |
| 207 | P-91 | Elite 8 | Earbud | Earbuds | Jabra |

`customer_key`, `product_key`, `order_date_key` on the fact each point at one of these tables.

### The signature of a dimension

- **Descriptive & textual** — attributes are human-readable labels (`Headsets`, `Madrid`, `B2B`), not just codes. Verbose is *good* — these become the row and column headers of every report.
- **Wide & shallow** — *many* descriptive columns, but relatively *few* rows. The opposite shape to the long, narrow fact.
- **The things you filter, group, and label by** — every attribute is a potential slice, drill, or axis.
- **Surrogate primary key** — a `_key` the fact's foreign key references (its own section, next-but-one).

### Facts vs dimensions, once more

Facts are *what happened and how much*; dimensions are *who, what, when, where, and what kind*. A dashboard is measures **from the fact**, sliced by attributes **from the dimensions**. This module is a tour of the dimension — its attributes, its keys, and the special shapes it takes.
