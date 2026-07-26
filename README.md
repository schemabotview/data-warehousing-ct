# data-warehousing-ct

Video-first content for the **Data Warehousing** concept, consumed at runtime by the [`graphl-render-app`](../graphl-render-app) app (the local mirror of the reference `graphl-movie`). Content only — per-section `.md` source (the source of truth, all section content), narration (`.tts` → `.wav`), authored one-screen slides (`.slide`: a `# Title`, `## ` sub-labels, short prose, fenced code, and `- `/numbered lists with **bold** key terms) distilled from the `.md`, and the wiring `manifest.json`. Nothing to build or run here. For the authoring contract and folder layout, see [`CLAUDE.md`](./CLAUDE.md).

This file is the **course outline** — the human-facing map of modules and sections. It is the plan we author against; the machine source of truth for structure is `manifest.json`.

**Status:** scaffolded + spine agreed (10 modules × ~10 sections — below). `manifest.json` wires the full spine (heading + md/slide/scene paths, `spine:true`, `role:hook` on each §01); `audio` and per-section `focus`/`highlight` get added as sections and scenes are authored. `audio/` is empty by design: the owner generates the `.wav`s from `tts/` via Colab, then the manifest `audio` fields get added back. **All 10 modules authored (100 sections)** — every section has its `.md` source of truth + distilled `.slide` + `.tts`. Scenes: `dw-architecture` (01, 09, 10), `star-schema` (02–06, 08), `datavault-model` (07). Modules 03–06 & 08 run on the Jabra Spain `FACT_SALES` galaxy model; module 07 (Data Vault) on `jabra-spain-datavault-model.dbml`; modules 09 & 10 are gap-filled (authored new — ETL/ELT loading, and cloud/MPP which also absorbs the `16 - SQL Optimization` tuning into a columnar-MPP context). `audio/` empty by design (owner generates `.wav`s via Colab); per-section `focus`/`highlight` get added as the scene node ids settle. **Authoring complete** — remaining work is `.wav` generation and scene `focus`/`highlight` wiring.

## Module spine (agreed — 10 modules × ~10 sections)

Re-authored from the ITC big-data data-modeling notes (`../ITC-bigdata/data-modeling-markdown/`, 19 numbered `.md` files + two `.dbml` models) into the video shape: **10 modules × ~10 tight teaching sections**, each a narrated slide + scene focus (≈ one page). The source is ~80% dimensional-modeling mechanics framed by a data-warehouse context; reshaped here into the recognizable **Data Warehousing** course — warehouse foundations at the front, the dimensional-modeling spine (facts → dimensions → schemas → SCD) in the middle, Data Vault and applied design after, and the two genuine gaps filled: **09 · Loading the Warehouse (ETL/ELT)** is authored new (the source barely touches it), and **10 · Cloud Data Warehouses & MPP** lands the whole course on a modern platform — it *absorbs* physical/query tuning (partitioning, clustering, columnar, star-join pruning) from the thin `16 - SQL Optimization` note into the cloud/MPP context where it actually lives, and adds the cloud-native beats the source never had. SQL-language depth stays in the separate `sql-ct` concept.

Scene assignment: **01, 09, 10 → `dw-architecture`** (the source → ETL → storage → marts → BI system map; module 10 reuses it to avoid scene proliferation — an optional dedicated `cloud-mpp` scene can be ported later if it reads cluttered), **07 → `datavault-model`** (hubs/links/satellites, sourced from `jabra-spain-datavault-model.dbml`), **all others → `star-schema`** (the shared `FACT_SALES` + dimensions master map, sourced from `jabra-spain-dw-model.dbml`).

| # | Module | Scene | Sections |
|---|---|---|---|
| 01 | Warehouse Foundations | `dw-architecture` | 10 |
| 02 | Normalization & Keys | `star-schema` | 10 |
| 03 | Fact Tables | `star-schema` | 10 |
| 04 | Dimension Tables | `star-schema` | 10 |
| 05 | Star & Snowflake Schemas | `star-schema` | 10 |
| 06 | Slowly Changing Dimensions | `star-schema` | 10 |
| 07 | Data Vault Modeling | `datavault-model` | 10 |
| 08 | Designing a Warehouse Model | `star-schema` | 10 |
| 09 | Loading the Warehouse — ETL & ELT | `dw-architecture` | 10 |
| 10 | Cloud Data Warehouses & MPP | `dw-architecture` | 10 |

Parked (not in the video set): the `17 - Dimensional Modeling — Interview Q&A` sheet (44 Q&A) — open-ended Q&A is a weak fit for the narrated-scene format, but it stays as a prose reference for authoring.

## Source map (which `.md` feeds which module)

| Module | Source files |
|---|---|
| 01 Warehouse Foundations | `01 - Data Warehouse & Data Modeling`, `02 - Data Warehouse & Data Lake` |
| 02 Normalization & Keys | `04 - Normalization & Denormalization`, `05 - Types of Keys`, `06 - Surrogate Key vs Natural Key` |
| 03 Fact Tables | `09 - Fact Table`, `10 - Fact Table Types`, `11 - Factless Fact Table` |
| 04 Dimension Tables | `07 - Keys, Dimensions & Facts`, `08 - Dimension Types` |
| 05 Star & Snowflake Schemas | `03 - Data Warehouse Model`, `13 - Star & Snowflake Schema` |
| 06 Slowly Changing Dimensions | `14 - Slowly Changing Dimensions`, `08 - Dimension Types` (SCD section) |
| 07 Data Vault Modeling | `12 - Fact, Dimension & Data Vault`, `jabra-spain-datavault-model.dbml` |
| 08 Designing a Warehouse Model | `15 - Designing Your First DW Model`, `jabra-spain-dw-model.dbml` |
| 09 Loading the Warehouse | *(new — gap filled)* |
| 10 Cloud Data Warehouses & MPP | `16 - SQL Optimization` (§6–§10 tuning) + *(new — cloud/MPP beats, §1–§5)* |

## Sections (agreed spine — final headings settle per module as authored)

### 01 — Warehouse Foundations (10) · `dw-architecture`

1. Why a data warehouse exists *(hook)*
2. What a data warehouse is
3. OLTP vs OLAP — the two workloads
4. Warehouse components — sources, ETL, storage, marts, BI
5. Data marts — departmental subsets
6. Warehouse vs data lake
7. Schema-on-write vs schema-on-read
8. The lakehouse — where the two converge
9. Inmon vs Kimball — top-down vs bottom-up
10. The modeling journey — conceptual, logical, physical

### 02 — Normalization & Keys (10) · `star-schema`

1. Normalization — why we split tables *(hook)*
2. The normal forms — 1NF, 2NF, 3NF
3. Splitting to 3NF — a worked example
4. Denormalization — trading redundancy for speed
5. Keys — super, candidate, primary, alternate
6. Composite keys
7. Foreign keys & referential integrity
8. Strong vs weak entities
9. Natural key vs surrogate key
10. Why warehouses prefer surrogate keys

### 03 — Fact Tables (10) · `star-schema`

1. What a fact table is — measures & grain *(hook)*
2. Grain — the most important decision
3. Additive, semi-additive & non-additive measures
4. Foreign keys to dimensions — the star center
5. Degenerate dimensions
6. Transactional fact tables
7. Periodic snapshot facts
8. Accumulating snapshot facts
9. Aggregate (summary) fact tables
10. Factless fact tables — events & coverage

### 04 — Dimension Tables (10) · `star-schema`

1. What a dimension is — the context you slice by *(hook)*
2. Attributes & hierarchies
3. Surrogate keys in dimensions
4. Conformed dimensions & the bus matrix
5. Role-playing dimensions
6. Junk dimensions
7. Degenerate dimensions revisited
8. The date dimension — the universal dimension
9. Fact vs dimension — the two building blocks
10. Common dimension design pitfalls

### 05 — Star & Snowflake Schemas (10) · `star-schema`

1. The star schema *(hook)*
2. Anatomy of a star — fact + dimensions
3. Why denormalized dimensions win for BI
4. The snowflake schema
5. Star vs snowflake — the trade-off
6. Galaxy / fact-constellation schemas
7. Physical design — a Sales star walkthrough
8. Keys in the physical model — PK, SK, FK, NK
9. Degenerate & derived columns in the fact
10. Choosing a schema for your workload

### 06 — Slowly Changing Dimensions (10) · `star-schema`

1. The problem — dimensions change over time *(hook)*
2. Type 0 — retain original
3. Type 1 — overwrite (no history)
4. Type 2 — add a row (full history)
5. Effective dates & the active flag
6. Type 3 — add a column (limited history)
7. Type 4 — history / mini-dimension tables
8. Type 6 — the hybrid (1 + 2 + 3)
9. Choosing an SCD type per attribute
10. Loading an SCD-2 dimension — the merge pattern

### 07 — Data Vault Modeling (10) · `datavault-model`

1. Why Data Vault exists — agility & auditability *(hook)*
2. The three building blocks — hubs, links, satellites
3. Hubs — the business keys
4. Links — the relationships
5. Satellites — descriptive history
6. Hash keys & load metadata
7. Raw vault vs business vault
8. Data Vault vs dimensional modeling
9. From Data Vault to star — the information mart
10. When to reach for Data Vault

### 08 — Designing a Warehouse Model (10) · `star-schema`

1. Kimball's 4-step design process *(hook)*
2. Step 1 — pick the business process
3. Step 2 — declare the grain
4. Step 3 — identify the dimensions
5. Step 4 — identify the facts
6. Worked example — modeling from a bill
7. Building the dimensions with surrogate keys
8. Building the order fact
9. The resulting star — end to end
10. Reading the model as DBML

### 09 — Loading the Warehouse — ETL & ELT (10) · `dw-architecture`

1. ETL vs ELT *(hook)*
2. Extract — pulling from source systems
3. Staging areas
4. Transform — cleaning, conforming, deduping
5. Load — full vs incremental
6. Change data capture (CDC)
7. Surrogate key generation & lookups
8. Loading dimensions before facts
9. Idempotent & restartable loads
10. Orchestration & data-quality checks

### 10 — Cloud Data Warehouses & MPP (10) · `dw-architecture`

1. Why warehouses moved to the cloud *(hook)*
2. MPP architecture — shared-nothing parallelism
3. Separation of compute & storage
4. The platforms — Snowflake, BigQuery, Redshift, Synapse
5. Data distribution — distribution & partition keys
6. Columnar storage & compression
7. Clustering, sort keys & micro-partitions / zone maps
8. Materialized views & result caching
9. Star-schema join optimization & partition pruning
10. Query best practices — before → after case study

> Note: SCD appears twice by design — a light "what it is" beat in module 04 (dimensions) and the full type-by-type treatment in module 06. Degenerate dimensions likewise get a fact-side mention (03) and a dimension-side recap (04). Module 10 folds physical/query tuning into a cloud/MPP context: partitioning/pruning (§5, §9), columnar & compression (§6), clustering/zone maps (§7), materialized views (§8), star-join optimization (§9) and best-practices/before→after (§10) all carry over from the old "Query & Physical Optimization" spine; B-tree "indexing strategies" is retired (replaced by micro-partitions/clustering in columnar MPP — itself the teaching point of §7).

## Reference material

- `../ITC-bigdata/data-modeling-markdown/` — the source notes: 19 numbered `.md` files (the prose source-of-truth) + `jabra-spain-dw-model.dbml` (star) + `jabra-spain-datavault-model.dbml` (Data Vault). **Prose reference only** — narration is authored fresh here and the owner generates the `.wav`s via Colab.
- `../apache-spark-ct/` — the sibling `-ct` template this repo mirrors (folder layout, manifest shape, authoring contract).
- `sql-ct` — the separate SQL-language concept (its own repo); DW query tuning here references SQL but does not re-teach it.
