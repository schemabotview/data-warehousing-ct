## The snowflake schema

The **snowflake schema** is an extension of the star where one or more dimensions are **normalized** — split back out into related **sub-dimension tables**. Where a star keeps each dimension flat, a snowflake lets a dimension branch into levels.

### From flat to branched

Take `DIM_PRODUCT`. In a star, `category` and `product_line` are columns on the one table. Snowflake it, and those become their own tables:

```
FACT_SALES ── DIM_PRODUCT ── DIM_CATEGORY ── DIM_PRODUCT_LINE
```

Now `DIM_PRODUCT` holds a `category_key` pointing at `DIM_CATEGORY`, which points at `DIM_PRODUCT_LINE`. The dimension has become a little multi-level tree — and the diagram, with dimensions sprouting sub-dimensions off the central fact, looks like a **snowflake**.

### A real one — the outrigger

The Jabra model has a genuine snowflake branch. `DIM_WAREHOUSE` doesn't store its own city and region — it carries a `geography_key` to a shared `DIM_GEOGRAPHY`:

```
FACT_SHIPMENT ── DIM_WAREHOUSE ── DIM_GEOGRAPHY
```

A normalized sub-dimension shared this way is called an **outrigger** — useful when the same geography is reused by several dimensions and you want to maintain it once.

### What it costs and saves

- **Saves space, cuts redundancy** — `product_line` is stored once in its own table, not repeated on every product row.
- **Costs joins and clarity** — a query for "sales by product line" must now chain `FACT → DIM_PRODUCT → DIM_CATEGORY → DIM_PRODUCT_LINE`. More joins, more foreign keys, more complex SQL, slower reads.

It's a **bottom-up** design (build the normalized pieces up) versus the star's **top-down** (start from the flat dimension).

> A snowflake normalizes dimensions into sub-tables — less redundancy and storage, but more joins and complexity. It's a star with its dimensions un-flattened; whether that's worth it is the next section.
