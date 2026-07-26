## Schema-on-write vs schema-on-read

The deepest difference between a warehouse and a lake is *when* you impose structure on the data.

**Schema-on-write** (the warehouse) applies the schema **as data is loaded**. You define tables, types and constraints up front; ETL transforms incoming data to fit, and anything that doesn't conform is rejected or fixed on the way in. Reads are then fast and predictable, and every consumer sees clean, consistent data — but you must model up front, and changing the schema later is work.

**Schema-on-read** (the lake) stores data **as-is** and applies structure **only when you query it**. Each reader interprets the raw files with whatever schema they need. Ingest is trivial and flexible — dump anything — but every reader carries the burden of parsing and cleaning, and quality is only as good as each query.

### ETL vs ELT

The two pair naturally with two pipeline orders:

- **ETL** — Extract → **Transform** → Load. Clean and conform *before* the data lands. Fits schema-on-write and the warehouse.
- **ELT** — Extract → Load → **Transform**. Land raw *first*, transform later inside the target (lake or modern cloud warehouse). Fits schema-on-read; leans on cheap storage and scalable compute.

### The trade-off

Schema-on-write buys **trust and speed** at read time for the cost of up-front modeling and rigidity. Schema-on-read buys **flexibility and cheap ingest** for the cost of messy, slower reads. Modern cloud platforms blur the line — load raw, then transform in place (ELT) — which is exactly what the lakehouse is built on (§08).
