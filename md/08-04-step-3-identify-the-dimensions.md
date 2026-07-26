## Step 3 — identify the dimensions

With the grain fixed at one order line, step 3 asks a single question and answers it many times: **"how do we describe an order line?"** Every distinct way to describe or slice it is a **dimension**.

### Work through who / what / when / where / why

Run the mnemonic against the grain — for each, *does this make sense for one order line?*

- **When** was it sold? → **Date** dimension.
- **Who** bought it? → **Customer** dimension.
- **What** was sold? → **Product** dimension.
- **Where / how** was it sold? → **Channel** dimension (Web, App, Amazon, Retail).
- **Why / under what deal?** → **Promotion** dimension.

Each answer is a dimension that attaches to the fact. The grain guides the whole exercise: anything true **at the order-line level** can be a dimension; anything that isn't, can't.

### The design habits that ride along

- **Surrogate key each one** — every dimension gets its own `_key` (module 04), which the fact will reference.
- **Reuse conformed dimensions** — don't build a new `DIM_DATE`; point at the one the enterprise already shares (module 04). This is how the new star joins the galaxy.
- **Be generous** — more dimensions, and richer attributes within them, mean more questions answerable. A thin set of dimensions is a thin warehouse.
- **Decide SCD intent** — note which attributes need history (customer address → Type 2) for later (module 06).

### The result of step 3

Five dimensions for the Jabra sales star: **Date, Customer, Product, Channel, Promotion** — each a table with a surrogate key, ready to be described and, in step 4, joined by the fact.

> Step 3 describes the grain: ask who/what/when/where/why of one order line, and each answer is a surrogate-keyed dimension — reusing conformed ones, and being generous, because dimensions are what you slice by.
