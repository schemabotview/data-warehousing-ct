## Orchestration & data-quality checks

A real load is **many steps across many tables**, run on a schedule. Two things make it dependable: **orchestration** to coordinate the steps, and **data-quality checks** to verify the result. Together they're what let people **trust** the warehouse.

### Orchestration — coordinate the steps

An **orchestrator** (Airflow, dbt, or a cloud service) runs the pipeline as a **DAG** — a graph of tasks wired in dependency order. It handles:

- **Order & dependencies** — extract → stage → transform → **dimensions → facts** (section 08), never out of sequence.
- **Scheduling** — run nightly, hourly, or triggered by data arrival.
- **Retries & recovery** — auto-retry a failed step, resume from checkpoints (section 09), and **alert** on failure.

The orchestrator is the conductor that makes a dozen interdependent jobs run reliably and repeatably.

### Data-quality checks — verify the result

Automated tests at each stage catch bad data **before** it reaches a report:

- **Row counts / volume** — did we load roughly the expected number of rows? (A sudden drop = a broken feed.)
- **Uniqueness & not-null** — dimension PKs unique, fact FKs all resolve, keys not null.
- **Range / domain** — values in valid bounds (no negative quantities, dates sane).
- **Reconciliation** — warehouse totals tie back to source totals (do the day's sales match the source?).

On failure, **fail the run or quarantine** the bad rows — never publish wrong numbers. Add **monitoring** for freshness and latency so you know the data is current.

### Closing the loop

Orchestration makes loads **reliable and repeatable**; data-quality checks make them **trustworthy**. Data now lands correct, on time, and verified — ready for the marts and BI, and for the cloud platforms that run all this at scale (module 10).

> Orchestration runs the pipeline as a scheduled DAG with dependencies, retries, and alerts; data-quality checks (counts, uniqueness, ranges, reconciliation) verify every load. Reliable *and* trustworthy — the foundation of a warehouse people believe.
