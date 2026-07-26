## ETL vs ELT

Modules 03–08 designed the tables. This module answers the next question: **how do they get filled?** Data doesn't appear in a warehouse — it's moved there from source systems by a **data pipeline**, built from three operations: **Extract**, **Transform**, **Load**. The order you run them in defines the two dominant architectures.

### ETL — transform before you land

Classic **ETL** is **Extract → Transform → Load**:

```
sources → EXTRACT → TRANSFORM (in an ETL engine) → LOAD into warehouse
```

Data is pulled from sources, cleaned and conformed in a **separate ETL engine**, and only the finished result is loaded. It suits a world where warehouse compute is **precious** — you don't want to burn it on transformation, so a dedicated engine does the heavy lifting first.

### ELT — land first, transform in place

Modern **ELT** is **Extract → Load → Transform**:

```
sources → EXTRACT → LOAD raw into warehouse → TRANSFORM (SQL, in-warehouse)
```

Raw data is loaded **first**, and transformation runs **inside the warehouse** as SQL. This works because the cloud warehouse is now the **most powerful, most scalable compute you have** (module 10) — so you push the work to where the data already sits.

### Same letters, different order — why it matters

- **ETL** — transform off to the side; warehouse only sees clean data; heavier tooling, less warehouse load.
- **ELT** — raw lands in the warehouse (great for auditability and reprocessing); transformation scales with cheap MPP compute; SQL-centric, tool-light.

ELT dominates the cloud era, but the *stages* are the same either way. The rest of this module walks each one — extract, staging, transform, load — then the warehouse-specific concerns: CDC, surrogate keys, load order, idempotency, and orchestration.

> ETL and ELT are the same three operations — extract, transform, load — in two orders. ETL transforms before loading (precious warehouse compute); ELT loads raw then transforms in-warehouse (cheap, scalable cloud compute) and now dominates.
