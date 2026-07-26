## Inmon vs Kimball — top-down vs bottom-up

The two foundational approaches to warehouse design come from Bill **Inmon** and Ralph **Kimball**. They disagree on where you start.

### Inmon — top-down

Build one **enterprise data warehouse** first: a single, integrated, **normalized** (3NF) repository for the whole organization, the corporate "single source of truth." Departmental **data marts** are then derived *from* it.

- **Pros** — enterprise-wide consistency, less redundancy, a robust foundation.
- **Cons** — slower and more expensive to deliver value; heavy up-front modeling.

### Kimball — bottom-up

Build **dimensional data marts** first, one business process at a time (sales, then inventory, …), each a **star schema**. The enterprise warehouse *emerges* as the union of these marts, tied together by **conformed dimensions** (shared Customer, Date, Product) via the **bus architecture**.

- **Pros** — fast to deliver, business-friendly, easy to query.
- **Cons** — needs discipline (conformed dimensions) or marts drift into silos.

### Which, and the modern take

Inmon suits complex enterprises that need a governed, integrated core; Kimball suits teams that need value fast and query simplicity. In practice many warehouses are **hybrid** — an integrated core (Inmon-ish, or a Data Vault) feeding Kimball-style dimensional marts for consumption. The dimensional model (Kimball) is what most BI users actually touch.
