## Choosing a schema for your workload

Three shapes, one decision. Star, snowflake, galaxy aren't a ranking — they answer different questions. Here's how to choose.

### The default — start with a star

For almost every warehouse and almost every dimension, **build a star**: a fact ringed by flat, denormalized dimensions. It's the fastest to query, the simplest to understand, and the friendliest to BI tools. Make the star your default and deviate only with a reason.

### Snowflake — only by exception, per dimension

Normalize a **specific** dimension into sub-tables only when one of these forces it:

- A **genuinely huge** dimension with a large, highly repetitive sub-hierarchy, where the storage saving is real.
- A sub-dimension **shared** across several dimensions — an **outrigger** you want to maintain once (Jabra's `DIM_GEOGRAPHY`).
- A **tool or governance** rule that mandates normalized reference data.

Snowflake the *one* dimension that needs it; leave the rest flat. It's a scalpel, not the default.

### Galaxy — as you add processes

You don't choose a galaxy so much as **grow into one**. Model your first business process as a star; when the next process arrives (shipping, payments), add it as **another star that reuses the conformed dimensions** you already built. Any multi-process enterprise ends up a galaxy — plan it with the bus matrix.

### The modern tilt

On columnar, MPP cloud platforms, joins are cheaper and storage cheaper still — which pushes even harder toward the **flat star** and away from snowflaking (module 10 returns to this).

> Default to a star; snowflake a single dimension only when size, sharing, or tooling demands it; and let conformed dimensions grow your stars into a galaxy as processes are added.
