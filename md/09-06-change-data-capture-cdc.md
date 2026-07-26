## Change data capture (CDC)

Incremental loading needs to know **what changed** in the source. **Change data capture (CDC)** is the set of techniques for identifying and capturing the **inserts, updates, and deletes** a source made — so you move only the **delta**, not the whole dataset. CDC is what makes efficient, near-real-time loading possible.

### The methods, weakest to strongest

- **Timestamp / high-water** — the source rows carry a `last_modified` column; grab everything changed since the last run. Simple and cheap, but **misses deletes** and can miss updates that don't touch the timestamp.
- **Trigger-based** — database triggers write every change to a change table. Accurate, but adds **overhead on the source** (every write does extra work).
- **Log-based CDC** — read the source database's **transaction log** (the redo log / binlog). Captures **every** insert, update, and delete, with **minimal source impact** — nothing extra runs on the operational system. This is the **gold standard**.
- **Snapshot diff** — compare a fresh full snapshot to the last one. Catches everything including deletes, but **expensive** — you extract the whole source to find the changes.

### Why CDC matters

Re-extracting an entire busy source every run simply **doesn't scale** — and it hammers the operational system. CDC replaces "pull everything" with "pull only the changes," which:

- **Feeds incremental loads** (section 05) — the delta is exactly what an incremental load writes.
- **Drives the SCD-2 merge** (module 06) — a captured change to a dimension row is what triggers expire-and-insert.
- **Enables near-real-time** — small, frequent deltas instead of one giant nightly batch.

> CDC captures a source's inserts, updates, and deletes so you load only the delta. Log-based CDC is the gold standard — complete and low-impact — and it's what powers incremental loads and SCD-2 merges at scale.
