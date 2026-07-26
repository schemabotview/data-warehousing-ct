## What a data warehouse is

A **data warehouse** is a central system that collects data from various source systems into a single location — making it easier to **access, analyze, and report** on the data. It is built for analysis and reporting rather than day-to-day transactions.

### Inmon's four characteristics

Bill Inmon's classic definition names four traits that make a warehouse different from an operational database:

- **Subject-oriented** — organized around the business subjects you analyze (sales, customers, products), not around the individual applications that produced the data.
- **Integrated** — data from many systems is cleaned and made consistent on the way in: one currency, one date format, one definition of "customer", so figures reconcile across sources.
- **Time-variant** — it keeps history. Where an operational system overwrites the current value, the warehouse retains the whole series, so you can compare this quarter to the same quarter last year.
- **Non-volatile** — data is loaded and then read, not continuously updated in place. Loads add new data; they don't overwrite what is already there — so a report run twice gives the same answer.

### What it's built from — components at a glance

A warehouse is a pipeline, not just a database. Its parts (covered in depth in §04):

- **Data sources** — the operational systems, files, and apps the data comes from.
- **ETL / ELT** — extract, transform, load: the process that moves and conforms the data.
- **Storage** — the warehouse database itself, where the curated data lands.
- **Metadata** — data about the data: definitions, lineage, and catalog.
- **Data marts** — subject-specific subsets serving one department.
- **Access / BI tools** — reporting, dashboards, and ad-hoc analysis on top.

### Types of warehouse

- **Enterprise Data Warehouse (EDW)** — one central warehouse for the whole organization.
- **Data mart** — a subset scoped to a single department (see §05).
- **Cloud data warehouse** — hosted and elastically scalable (BigQuery, Redshift, Snowflake); covered in Module 10.
