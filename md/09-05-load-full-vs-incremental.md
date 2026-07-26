## Load — full vs incremental

**Load** is the final move: writing the transformed data into the warehouse tables. The key decision is *how much* to write each run — the whole table, or only what changed.

### Full load

A **full load** truncates the target and **reloads it entirely** every run.

- **Pros** — dead simple, and **self-correcting**: whatever the last state, you rebuild it fresh, so drift and past errors wash out.
- **Cons** — **expensive** and slow at scale; you can't truncate-and-reload a billion-row fact nightly.
- **Use for** — small dimensions and **reference tables**, where reloading everything is cheap.

### Incremental load

An **incremental load** writes only the **new or changed rows** since the last run.

- **Pros** — **efficient and scalable**; only touches the delta. Essential for large facts.
- **Cons** — more complex: you must **know what changed** (CDC, section 06) and handle updates, not just inserts.
- **Use for** — big fact tables and large dimensions.

### The high-water mark

Incremental loading needs a bookmark: track the **last-loaded timestamp or id** (the *high-water mark*), and next run pull only rows **beyond** it. Advance the mark **only on success** — that's what makes a re-run safe (section 09).

### Facts append, dimensions merge

The two table types load differently:

- **Facts** — usually **append-only** inserts (a new event is a new row).
- **Dimensions** — **upsert / merge**, because an existing member may change → the SCD-2 merge from module 06.

Most warehouses mix both strategies: **full** for the small stuff, **incremental** for the big stuff.

> Load writes transformed data to the warehouse — full (truncate-and-reload, simple, for small tables) or incremental (only the delta, scalable, for big ones), tracked by a high-water mark. Facts append; dimensions merge.
