## Why warehouses moved to the cloud

For decades a data warehouse was a **big fixed box** — an on-premises appliance you sized for peak load, bought up front, and lived with. Capacity was provisioned for the busiest hour and idle the rest of the time; scaling meant buying more hardware; a team tuned indexes and managed disks. The cloud data warehouse changed both the **economics** and the **architecture**.

### What the cloud changed

- **Elasticity** — scale compute **up and down on demand** and pay only for what you use. No more provisioning for peak and paying for idle.
- **Separation of compute & storage** — store data cheaply and durably, and spin up compute **only when a query runs** (section 03).
- **MPP under the hood** — massive parallelism across many nodes makes scanning billions of rows fast (section 02).
- **Fully managed** — no hardware, patching, or manual index tuning; the platform handles storage, distribution, and much of the optimization.
- **Opex, not capex** — per-second/per-query billing replaces a big up-front appliance purchase.

### The modern shape

Today's warehouse is a **cloud, MPP, columnar, compute/storage-separated** system. That shape doesn't just change *where* it runs — it changes **how you make it fast**. The old lever, the **B-tree index**, is gone; you tune with **distribution, columnar storage, clustering, pruning, and caching** instead.

### What this module does

This is the course's landing. We'll cover the cloud/MPP foundations (sections 02–05), then fold the whole of **physical and query tuning** into that context — columnar storage, clustering and zone maps, materialized views and caching, star-join pruning — closing with a **before → after** query case study.

> Warehouses moved to the cloud for elasticity, cheap separated storage, MPP speed, and managed operations. The modern platform is cloud + MPP + columnar — and you tune it with distribution, clustering, pruning, and caching, not indexes.
