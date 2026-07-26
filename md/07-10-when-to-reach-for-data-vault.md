## When to reach for Data Vault

Data Vault is powerful, but it isn't free — many small tables, more joins, more ETL, and a mart layer to build on top before anyone can query. So it's an **architectural choice**, not a default. Reach for it when its strengths pay for that complexity.

### Reach for Data Vault when…

- **Many source systems** must be integrated into one enterprise view. This is the vault's core strength — hubs merge the same entity across sources.
- **Sources and requirements change often.** Agility means absorbing change by *adding* tables, never re-engineering — you stop paying for constant redesign.
- **Audit, compliance, or lineage is required** — finance, healthcare, regulated industries. Every row is stamped with source and load time, and nothing is ever overwritten.
- **Scale demands parallel loading.** Deterministic hash keys let hubs, links, and satellites load independently on MPP platforms.
- **The warehouse is long-lived** — an enterprise data warehouse meant to last years and evolve, where future-proofing outweighs upfront effort.

### Don't bother when…

- The project is a **small, stable, single-source** data mart.
- You need a **quick analytics** deliverable and requirements are settled.
- There's **no audit or integration** burden to justify the extra layer.

In those cases the vault is over-engineering — model a **star directly** and move on.

### The rule of thumb

> Use Data Vault as the **enterprise integration layer** feeding many star marts — when you have many sources, constant change, and audit demands. For a focused, stable mart, skip it and go straight to a star.

That closes the modeling arc: normalize for OLTP (02), denormalize into stars (03–05), version with SCD (06), and — when the enterprise demands it — integrate underneath with a Data Vault (07), serving stars on top.
