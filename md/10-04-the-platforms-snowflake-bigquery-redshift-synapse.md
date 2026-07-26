## The platforms — Snowflake, BigQuery, Redshift, Synapse

Four cloud warehouses dominate. All are **MPP + columnar** and run star schemas beautifully; they differ mainly in **how compute/storage is handled** and **how much tuning you do**.

### Snowflake

Multi-cloud (runs on AWS, Azure, GCP). True **compute/storage separation** with **virtual warehouses** — independent compute clusters over shared storage. Data sits in **micro-partitions** with **automatic clustering** — minimal manual tuning. Known for ease of use and workload isolation.

### BigQuery (Google)

**Serverless** — no clusters to size or manage; compute is allocated per query (slots). Columnar storage (Capacitor), with **partitioning + clustering** columns you declare. Billing is **per bytes scanned** (or reserved slots) — which makes "scan less" a direct cost lever.

### Redshift (AWS)

AWS's MPP warehouse, the most **manually tuned** of the four: you choose **distribution keys** (DISTKEY) and **sort keys** (SORTKEY) to control layout. **RA3** nodes added compute/storage separation. Deep AWS integration.

### Synapse (Azure) / Fabric

Azure's MPP warehouse (formerly SQL DW). Explicit **distribution** strategies — hash, round-robin, replicated. Integrates with the Azure / Microsoft Fabric analytics stack.

### The common thread

- **All** are columnar, MPP, cloud — a **star schema is the right model on every one**.
- The axis of difference is **automatic vs manual tuning**: Snowflake and BigQuery automate distribution/layout; Redshift and Synapse expose distribution and sort keys for hands-on control.
- **Choose by** your cloud, your tuning appetite, and the pricing model (per-second compute vs per-byte scanned).

> Snowflake, BigQuery, Redshift, and Synapse are all MPP + columnar and run stars well. They differ in compute/storage handling and how much you tune — automatic (Snowflake, BigQuery) vs explicit distribution/sort keys (Redshift, Synapse).
