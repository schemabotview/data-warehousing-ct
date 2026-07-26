## Warehouse vs data lake

A **data warehouse** stores **structured, processed** data — cleaned, transformed and modeled for analysis and BI. A **data lake** stores **raw data in its native format** — structured, semi-structured and unstructured — kept cheaply until someone needs it.

### Side by side

- **Data** — warehouse: structured, processed; lake: raw, all formats.
- **Schema** — warehouse: schema-on-write (defined up front, on load); lake: schema-on-read (defined when queried).
- **Processing** — warehouse: ETL (transform then load); lake: ELT (load then transform).
- **Users** — warehouse: business analysts & BI; lake: data scientists & engineers.
- **Workload** — warehouse: OLAP, reporting & dashboards; lake: big data, ML, exploration.
- **Cost** — warehouse: higher (compute + curated storage); lake: lower (cheap object storage).
- **Quality** — warehouse: high, curated & consistent; lake: variable, raw.

### When to use which

Use a **warehouse** when you know the questions: recurring reports, dashboards, trusted metrics. Use a **lake** when you don't yet: exploratory analytics, machine learning, or raw data you want to keep now and shape later. Many organizations run both — raw lands in the lake, curated subsets flow into the warehouse (and the lakehouse, §08, merges the two).
