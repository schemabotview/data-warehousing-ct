## The problem — dimensions change over time

A fact, once written, is history — a sale that happened is done. But **dimensions keep changing**. Ana moves from Madrid to Barcelona. A product is re-priced, or shifts category. A customer flips from B2C to B2B. These changes are *slow* — occasional and unpredictable, not a stream — which is exactly why they're tricky.

### The question that has no default answer

Suppose Ana bought a headset **last year**, while living in Madrid, and today she lives in **Barcelona**. Now you run "**sales by region**". Which region should that old sale count toward?

- **Barcelona** — where she lives *now* (report the world *as it is*).
- **Madrid** — where she lived *when the sale happened* (report the world *as it was*).

**Both are correct** — they answer different questions. "Total customers currently in each region" wants *now*; "revenue by region in 2025" wants *then*. A warehouse must be able to serve whichever the business needs — and that is a **modeling decision**, made before the change ever arrives.

### SCD — the framework for that decision

**Slowly Changing Dimensions (SCD)** is the framework for **managing and tracking changes to dimension data over time**. The different strategies are the **SCD types**, and the axis that separates them is one thing: **how much history you keep.**

- Keep *none* — overwrite, and the past is rewritten (Type 1).
- Keep *all* — add a new row per change, and every fact stays linked to the version true at its time (Type 2).
- Keep *some* — a previous-value column, a hybrid, a side table (Types 3, 4, 6).

### The mechanism underneath

All of this rides on the **surrogate key** from module 04. Because the fact points at a surrogate, not the natural key, a dimension can hold *several versions of the same business entity* — same `customer_id`, different `customer_key`s — and each fact row points at the one that was current when it happened.

> Dimensions change slowly, and "what was true then vs. now" has no universal answer. SCD is how you decide, per case, how much history to keep — and this module walks every type.
