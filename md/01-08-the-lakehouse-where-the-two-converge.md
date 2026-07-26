## The lakehouse — where the two converge

A **lakehouse** is an architecture that puts warehouse-style structure and management **directly on top of cheap lake storage** — one system instead of a separate lake and warehouse.

### The problem it solves

The classic split has costs: data is copied twice (lake, then warehouse), the copies drift, and you pay to store and move it more than once. Teams want the lake's cheap, open, all-format storage *and* the warehouse's reliability, performance and SQL.

### How it works

A lakehouse keeps data in open columnar files (like Parquet) on object storage, and adds a **transactional table layer** on top — Delta Lake, Apache Iceberg, or Apache Hudi. That layer brings warehouse guarantees to the lake:

- **ACID transactions** — reliable concurrent reads and writes.
- **Schema enforcement & evolution** — structure when you want it, changeable when you need it.
- **Time travel** — query the table as of an earlier version.
- **BI + ML on one copy** — SQL analytics and data science read the same tables.

### Why it matters

The lakehouse collapses "schema-on-read vs schema-on-write" into a spectrum on a single store: land raw, then curate in place with ELT, without copying data into a separate warehouse. It's the model behind platforms like Databricks and the open table formats.
