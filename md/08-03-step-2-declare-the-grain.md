## Step 2 — declare the grain

With the process chosen, the second step is to **declare the grain** — state, in plain business terms, **what one fact row represents** — *before* you list a single dimension or measure. This is the same "most important decision" from module 03, now sitting at its rightful place in the design sequence.

### Say it as a sentence

For the sales-order process, the grain is:

> **one row per product per order** — i.e. one **order line**.

That's it — a sentence a business person would recognise. Not "one row per order" (that hides the individual products), and not "one row per day" (that's an aggregate). One order line.

### Choose the finest grain the source allows

Prefer the **lowest, most atomic** level the data supports — here, the order line. Atomic grain is **maximum flexibility**: from order lines you can roll *up* to per-order, per-day, per-region, per-category totals — any summary later. You can never roll *down*, so capturing detail now is what keeps future questions answerable.

### Why it must come before steps 3 and 4

The grain is the **contract** the next two steps are held to:

- **Dimensions (step 3)** — only things true *at the order-line level* can attach.
- **Facts (step 4)** — only measures meaningful *per order line* belong.

Declare it wrong, or vaguely, and steps 3–4 have nothing solid to test against. And the rule stands: **one grain per fact table** — never mix order lines and order totals in the same fact.

> Step 2 declares the grain in one business sentence — "one order line" — at the finest level the source supports. It's the contract that makes identifying dimensions and facts possible; fix it before going on.
