## Loading an SCD-2 dimension — the merge pattern

Type 2 is only as good as the ETL that maintains it. Each load has to compare the incoming source against the current dimension and apply the **expire-and-insert** dance correctly. That logic is the **merge pattern**.

### The four cases of a daily load

Match each incoming source row to the dimension on the **natural key** (`customer_id`), then decide:

1. **New natural key** (customer we've never seen) → **INSERT** a new row: fresh surrogate key, `effective_date = today`, `expiry_date = 9999-12-31`, `is_current = 'Y'`.
2. **Existing key, a tracked attribute changed** → the **expire-and-insert**:
   - **UPDATE** the current row: `expiry_date = today`, `is_current = 'N'`.
   - **INSERT** a new version: new surrogate, `effective_date = today`, `expiry = 9999-12-31`, `is_current = 'Y'`.
3. **Existing key, no tracked change** → **do nothing** (an untracked column may still be a Type 1 overwrite).
4. **Existing key, missing from source** → depends on policy (leave as-is, or close it out).

### Detecting change cheaply

You don't compare every column by hand — you compare a **hash** of the tracked attributes (old row vs. incoming). Hash differs → it's a change (case 2); hash matches → no change (case 3). Fast, and easy to extend.

### One statement, or one tool

Most engines express cases 1–3 as a single SQL **`MERGE`** (`WHEN MATCHED … UPDATE`, `WHEN NOT MATCHED … INSERT`). Modern stacks often delegate the whole pattern to a tool — a **dbt snapshot**, or a warehouse's native change-tracking — so you declare "track these columns, Type 2" and it emits the merge.

### It must be idempotent

Re-running today's load must **not** create duplicate versions. Keyed on natural key + effective date and gated by the hash check, a correct merge is **idempotent** — safe to re-run after a failure (module 09's restartable-loads principle).

> The SCD-2 load is a merge: insert new keys, expire-and-insert changed ones, skip the unchanged — detected by a hash, written as `MERGE` or a snapshot tool, and idempotent so it's safe to re-run.
