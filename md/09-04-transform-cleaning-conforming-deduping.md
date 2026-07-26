## Transform — cleaning, conforming, deduping

**Transform** is where raw source data becomes **warehouse-ready** — the stage that turns many messy inputs into one trustworthy, conformed dataset. It's the substance of the pipeline; everything else moves data, this stage *improves* it.

### The core operations

- **Clean** — fix the mess: handle nulls, correct bad types, reject invalid values, and **standardise formats** — dates to one format, currencies to EUR, casing and spelling to one convention.
- **Conform** — map each source's codes to the **shared enterprise values** and conformed dimensions (module 04). Three systems sending `'M'`, `'Male'`, `'1'` all become one agreed `'Male'`. This is what makes "region" mean the same thing everywhere.
- **Deduplicate** — collapse duplicate records into one. The *same* customer arriving from web, POS, and ERP becomes a **single** row (the integration the hub did in module 07, done here in the pipeline).
- **Derive** — compute derived measures (`line_total = qty × price − discount + tax`) and apply business rules.
- **Integrate** — join and merge the cleaned sources into unified, warehouse-shaped rows.

### Where the "single version of truth" is enforced

Transform is where **data quality** and **consistency** are won or lost. A warehouse's whole promise — one trustworthy number everyone agrees on — is manufactured here, by conforming and deduping many sources into one clean picture.

### ETL vs ELT, again

Same operations, different place: in **ETL** they run in the engine *before* load; in **ELT** they run as **SQL inside the warehouse** *after* raw data lands. dbt is the popular ELT choice — transformation expressed as version-controlled SQL models.

> Transform makes raw data warehouse-ready: clean it, conform codes to shared values, deduplicate across sources, derive measures, integrate. It's where the single version of truth is built — in an ETL engine, or as in-warehouse SQL.
