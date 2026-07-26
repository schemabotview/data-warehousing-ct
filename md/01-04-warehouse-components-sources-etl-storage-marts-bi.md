## Warehouse components — sources, ETL, storage, marts, BI

A data warehouse is an end-to-end pipeline. Data flows left to right through six kinds of component.

- **Data sources** — where raw data comes from: operational OLTP databases, flat files and APIs, and SaaS apps like CRM and ERP. Each has its own schema and quirks.
- **ETL / ELT** — the movement layer: **Extract** from sources, **Transform** (clean, deduplicate, conform to common definitions), and **Load** into the warehouse. A **staging area** holds raw extracts before transformation. (ELT swaps the order — load first, transform inside the warehouse; see §07.)
- **Storage** — the warehouse database itself: the curated, integrated tables (facts and dimensions) optimized for analytical reads.
- **Metadata** — "data about the data": table and column definitions, data types, lineage (where each value came from), and the business glossary. The catalog that makes the warehouse understandable and governable.
- **Data marts** — subject-specific subsets of the warehouse serving one department (sales, finance, marketing), so each team queries a focused slice. (See §05.)
- **Access / BI tools** — the consumption layer: dashboards and reports, ad-hoc / OLAP analysis, and data-science / ML workloads that read from the warehouse and marts.

### How they fit together

Sources feed ETL, which lands data in staging and then loads clean data into storage. Metadata describes everything along the way. Marts carve focused subsets out of storage, and BI tools sit on top of both — turning the curated data into decisions.
