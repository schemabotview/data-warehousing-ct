## Grain — the most important decision

Before you choose a single measure or dimension, you answer one question: **what does one row of this fact table represent?** That answer is the **grain** — the level of detail — and it is the most consequential decision in dimensional modeling. Every other choice flows from it.

Kimball's rule is blunt: **declare the grain first, and declare it in business terms.** Not "one row per day" but "**one row per product per order**" — a sentence a business person would recognise.

### Why it dominates everything

Once the grain is fixed, two things follow automatically:

- **The dimensions** — the grain names them. "One order line" means every row can carry a date, a customer, a product, a channel. Anything true at that level of detail can attach as a dimension.
- **The measures** — only numbers that are true *at that grain* belong on the fact. `quantity` and `line_total` make sense per order line. An order-level shipping charge does not — it belongs to a coarser grain.

### Prefer the finest grain

Choose the **lowest, most atomic** grain the source data supports — usually one row per transaction line. Fine grain is not wasteful; it is **flexible**. From atomic order lines you can roll *up* to daily, monthly, per-region, per-category totals — any summary the business will ever ask for. You can never roll *down*: if you store only daily totals, the individual sales are gone forever, and the questions you didn't anticipate can't be answered.

### The cardinal sin — mixing grain

Every row must mean the **same** thing. If FACT_SALES holds order *lines* on some rows and order *totals* on others, every `SUM` double-counts and no one can trust a number. One fact table, one grain. If you need a different level of detail — say daily store totals — that is a *separate* fact table (an aggregate), not extra rows in this one.

> Declare the grain in one sentence. If you can't, you don't yet understand the process you're modeling.
