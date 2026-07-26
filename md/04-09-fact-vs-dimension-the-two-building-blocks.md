## Fact vs dimension — the two building blocks

Step back from all the special types and the whole dimensional model reduces to **two kinds of table**. Every table in a star is either a **fact** or a **dimension**, and telling them apart is the core skill of the discipline.

### The two roles

- **Fact** — the *measurements* of a business process. Numeric, **additive**, one row per event at a fixed grain. **Deep** (many rows, growing forever), **narrow** (few columns), all foreign keys and measures. `FACT_SALES`.
- **Dimension** — the *context* for those measurements. Textual, **descriptive**, one row per entity. **Wide** (many attributes), **shallow** (relatively few rows), a surrogate primary key. `DIM_CUSTOMER`, `DIM_PRODUCT`, `DIM_DATE`.

| | Fact | Dimension |
|--|------|-----------|
| Holds | measures (numbers) | attributes (context) |
| Answers | *what happened / how much* | *who, what, when, where* |
| Shape | long & narrow | wide & shallow |
| Grows | fast, forever | slowly |
| Key | FKs to dimensions | surrogate PK |
| Used to | aggregate | filter, group, label |

### The test — sum it, or slice by it?

When you meet a column, ask one question: **would you `SUM` it, or `GROUP BY` it?** A thing you add up is a **measure** (fact); a thing you slice by is an **attribute** (dimension).

A number can fall on either side, and that trips people up. `line_total` on the fact is a **measure** — you sum it. But `list_price` on `DIM_PRODUCT` is an **attribute** — you filter and group by it ("products over €200"), you don't add prices together. Same data type, opposite role. The role, not the type, decides the table.

> Two tables build every star: facts you aggregate, dimensions you slice by. Get a column onto the right one — sum it or group by it — and the model designs itself.
