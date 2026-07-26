## OLTP vs OLAP — the two workloads

Databases serve two fundamentally different jobs, and a data warehouse exists because one database can't do both of them well.

**OLTP — Online Transaction Processing** runs the business, moment to moment. It records transactions: place an order, update an account, book a seat. The work is many tiny, concurrent reads and writes, each touching a few rows, and it must be fast and correct *right now*.

**OLAP — Online Analytical Processing** analyzes the business. It answers big questions over lots of history: total revenue by region and quarter, year-over-year growth, which products sell together. The work is a few large, read-heavy queries that scan and aggregate millions of rows.

### Side by side

- **Purpose** — OLTP runs operations; OLAP supports decisions.
- **Operations** — OLTP: insert / update / delete of single records; OLAP: large aggregate reads.
- **Data** — OLTP: current, detailed; OLAP: historical, summarized.
- **Schema** — OLTP: highly normalized (3NF) to avoid update anomalies; OLAP: denormalized (star) for fast reads.
- **Queries** — OLTP: simple, known, high volume; OLAP: complex, ad-hoc, low volume.
- **Users** — OLTP: many app users / clerks; OLAP: analysts and decision-makers.
- **Sizing** — OLTP: gigabytes, tuned for throughput and low latency; OLAP: terabytes+, tuned for scan speed.

### Why keep them apart

Running heavy OLAP scans on the OLTP system competes for the same resources and slows the live application. The two access patterns also want opposite physical designs — normalized versus denormalized. So we copy data out of the OLTP systems into a warehouse built for OLAP, and let each do what it's good at.
