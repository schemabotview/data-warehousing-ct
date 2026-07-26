## Surrogate key generation & lookups

Module 08 mentioned "look the natural key up to get the surrogate." This section is that mechanic in full — the **surrogate-key pipeline** is the heart of dimensional loading, the thing that wires every fact row to the right dimension rows.

### Generation — dimensions mint their keys

When a **new** dimension member is loaded, the load **generates its surrogate key** — from a database **sequence**, an **identity/auto-increment** column, or a **hash** (the Data Vault approach, module 07). Dimensions **own** their keys; the natural key rides along as an attribute. Reserve key `0` (or `-1`) for the **"Unknown" member** (module 04).

### Lookup — facts resolve keys at load

A source fact row carries only **natural keys** (`C-4471`, `P-88`). Before writing the fact, the load **looks each natural key up in its dimension** to fetch the surrogate FK:

```
C-4471  → DIM_CUSTOMER → customer_key = 1101
P-88    → DIM_PRODUCT  → product_key  = 204
```

Those surrogates become the fact row's foreign keys. Every dimension key on the fact goes through this translation.

### Handling the misses

- **Natural key not found** → either load its dimension **first** (section 08), or point the fact at the **"Unknown" member** and backfill later (a *late-arriving fact*).
- **SCD-2 dimensions** → don't just grab `is_current`; look up the **version that was current at the fact's event date** (`WHERE event_date BETWEEN effective_date AND expiry_date`, module 06). That's what keeps a historical sale linked to the customer's *then*-address.

### Why it's the crux

This lookup is what makes the star **join correctly** and preserves history. Get it right and every fact FK resolves to the exact dimension version it should; get it wrong and reports silently mis-attribute.

> The surrogate-key pipeline generates keys when dimensions load, then resolves each fact's natural keys to surrogate FKs at load time — using the SCD-2 version current at the event date, and the Unknown member for misses.
