## Idempotent & restartable loads

Pipelines fail mid-run — a network blip, a bad file, a crashed node. What matters is what happens **next time you run it**. Two properties make failure survivable: **idempotency** and **restartability**.

### Idempotent — safe to run twice

A load is **idempotent** if running it **twice produces the same result as running it once** — no duplicate rows, no double-counted measures. Without it, a retry after a partial failure **double-loads** and quietly corrupts every total. Techniques:

- **MERGE / upsert** keyed on natural key (+ effective date for SCD-2, module 06) instead of blind `INSERT` — re-running just re-matches, it doesn't re-add.
- **Delete-then-insert a partition** — reload *today's* partition wholesale; re-running replaces it, never appends.
- **Deterministic keys** — a hash key (module 07) is the same on every run, so duplicates collapse instead of multiplying.

### Restartable — resume after failure

A load is **restartable** if it can **pick up from where it stopped** rather than redo everything. What enables it:

- **Staging as a checkpoint** — re-run transforms from staged data, no re-extract (section 03).
- **Per-step state** — each step records success, so orchestration resumes at the first incomplete one (section 10).
- **High-water mark advances only on success** — a failed run doesn't move the bookmark, so the next run re-pulls exactly the un-loaded delta (section 05).

### Why it's the operational backbone

Together these turn a failure from a **crisis** into a **re-run**. You don't hand-repair half-loaded tables; you just launch the job again and trust it to converge. This is the same idempotency the SCD-2 merge relied on (module 06) — now the whole pipeline's guarantee.

> Make loads idempotent (re-running never duplicates — via merge, partition reload, or deterministic keys) and restartable (resume from staging/checkpoints, advance the high-water mark only on success). Then a failure is just a safe re-run.
