## Galaxy / fact-constellation schemas

A single star models **one** business process. But a real enterprise runs many — Jabra doesn't just *sell*; it *ships*, *takes payments*, and *tracks stock*. Each is its own fact. Put several stars together, sharing dimensions, and you have a **galaxy schema** — also called a **fact constellation**.

### Multiple facts, shared dimensions

A galaxy is **two or more fact tables that share conformed dimensions**. The Jabra warehouse is exactly this:

```
              DIM_DATE   DIM_PRODUCT   DIM_CUSTOMER
                 │            │             │
   FACT_SALES ───┼────────────┼─────────────┤
   FACT_SHIPMENT ┼────────────┼─────────────┤
   FACT_PAYMENTS ┴────────────┴─────────────┘
```

`FACT_SALES`, `FACT_SHIPMENT`, `FACT_PAYMENTS`, and `FACT_INVENTORY_SNAPSHOT` each have their own grain and measures, but they **reuse the same** `DIM_DATE`, `DIM_CUSTOMER`, `DIM_PRODUCT`. The shared dimensions are what stitch the separate stars into one connected model — a constellation of stars linked at their shared points.

### Why it's the real-world shape

- **Every enterprise is multi-process.** One fact can't hold selling *and* shipping — different grains, different measures. So you build a fact per process.
- **Conformed dimensions integrate them** (module 04). Because all facts share the *same* `region` and `product_line`, you can compare and drill **across** processes: revenue, units shipped, and cash collected, all "by region, by month", side by side.
- **The bus matrix is the plan.** Its rows (processes) become the facts; its shared columns (dimensions) become the conformed dimensions every star reuses.

### How stars grow into a galaxy

You don't design a galaxy up front and all at once. You build **one star**, then add the next process as **another star** that reuses the dimensions already built. Conformed dimensions make each addition snap into the whole.

> A galaxy is many stars sharing conformed dimensions — the natural shape of a whole-enterprise warehouse. Build stars one process at a time; the shared dimensions make them one constellation.
