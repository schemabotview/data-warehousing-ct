## Effective dates & the active flag

Type 2 keeps many versions of a row — so each version needs to say **when it was valid** and **which one is live now**. Three control columns do that job. They're the machinery that makes full history usable.

### The three columns

- **`effective_date`** (valid-from) — the day this version *became* current.
- **`expiry_date`** (valid-to) — the day this version *stopped* being current.
- **`is_current`** (active flag) — a `Y`/`N` boolean marking the one live row.

Ana's two versions, fully stamped:

| customer_key | city | effective_date | expiry_date | is_current |
|--------------|------|----------------|-------------|------------|
| 1101 | Madrid | 2021-06-01 | 2026-03-31 | N |
| 1108 | Barcelona | 2026-04-01 | 9999-12-31 | Y |

The versions form a **contiguous timeline** — one ends the day before the next begins — with no gaps and no overlaps.

### The high-date trick

The current row's `expiry_date` is set to a **high date** — `9999-12-31` — not left NULL. It means "still valid, indefinitely." Using a real far-future date instead of NULL lets a single **`BETWEEN`** test work for every row without special-casing nulls.

### How you query it

- **Current view** — the latest version of everyone: `WHERE is_current = 'Y'`. The active flag is a shortcut so you don't do date math for the common case.
- **Point-in-time ("as of" date `D`)** — the version valid on a given day: `WHERE D BETWEEN effective_date AND expiry_date`. This is how a fact links to the *right* version — join on the surrogate, or resolve the version by the fact's date.

### Two consistency rules ETL must hold

- Exactly **one** current row per natural key (`is_current = 'Y'`).
- The timeline is **continuous**: each new version's `effective_date` picks up where the prior `expiry_date` left off.

> `effective_date`, `expiry_date`, and `is_current` turn a pile of versioned rows into a queryable timeline: the flag finds "now" fast, the date range answers "as of then", and the high date keeps the current row open without NULLs.
