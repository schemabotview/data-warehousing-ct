## Staging areas

Between extract and the warehouse sits the **staging area** — a **landing zone** where raw extracted data rests temporarily before it's transformed. It looks like plumbing, but it's what makes a pipeline robust.

### What staging is

A set of tables (or files) that hold the **raw output of extract**, usually mirroring the source's shape — schema-light, minimally typed, uninterpreted. Data lands here first; transformation reads *from* here, not from the source.

### Why it earns its place

- **Decouples extract from transform.** Extract dumps to staging fast and disconnects — the **source system is released** quickly, minimising its load. Transformation then proceeds on its own schedule.
- **A safe workspace.** Clean, conform, and join in staging without ever touching the live source or the published warehouse.
- **A restart checkpoint.** If transform fails, you **re-run from staging** — no need to re-extract from the source (ties to restartable loads, section 09).
- **A raw audit copy.** Keeping the untouched extract enables **reprocessing** — replay transforms after a logic fix — and proves what arrived.

### Transient or persistent

- **Transient staging** — truncated and reloaded each run; just a working buffer.
- **Persistent staging** — retains history of every extract; a durable raw archive (close in spirit to a Data Vault raw layer, module 07).

### Staging in ETL vs ELT

In **ETL**, staging lives in the ETL engine, *before* the warehouse. In **ELT**, staging is often **raw tables inside the warehouse itself** — you `LOAD` into staging tables, then `TRANSFORM` them into the star with SQL. Same role, different address.

> The staging area is the pipeline's landing zone: it decouples extract from transform, gives a safe workspace, a restart checkpoint, and a raw audit copy — transient or persistent, in the ETL engine or inside the warehouse.
