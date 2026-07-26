## Why Data Vault exists — agility & auditability

The star schema (modules 03–06) is a superb *reporting* model — but point it straight at a messy, changing enterprise and its weakness shows: it's **rigid**. A new source system, a new relationship, a change of grain can force you to re-engineer facts and dimensions and reload history. And history itself is *bolted on* through SCD. For a large, multi-source, long-lived warehouse, that brittleness is a real cost.

**Data Vault** is a different modeling methodology (Dan Linstedt's), built not for the query layer but for the **raw integration layer** underneath it. It's engineered for three things a star struggles with:

- **Agility** — absorb a new source or relationship by *adding* tables, never restructuring existing ones. The model grows; it doesn't get rebuilt.
- **Auditability** — every row is stamped with **where it came from** and **when it loaded**, and nothing is ever overwritten. The vault is the immutable **system of record** — you can always prove exactly what a source said, and when.
- **Scalable, parallel loading** — the structure lets hubs, links, and satellites load **independently and in parallel**, so it scales to huge volumes and MPP platforms.

### The core idea

Data Vault separates three concerns that a star fuses together: **business keys**, **relationships**, and **descriptive/historical context**. Split into three table types — **hubs, links, satellites** — each can evolve on its own. New attribute? New satellite. New relationship? New link. New entity? New hub. Existing structures are untouched.

### It's a layer, not a rival

Crucially, Data Vault does **not** replace the star. It sits *beneath* it: the vault **integrates and preserves** raw data from every source, and a star **information mart** is built **on top** for people to query. You get the vault's flexibility and audit trail *and* the star's fast, friendly reporting.

> Data Vault exists to make the enterprise integration layer agile and auditable — add-only structures, full history, source lineage on every row — feeding star marts rather than replacing them.
