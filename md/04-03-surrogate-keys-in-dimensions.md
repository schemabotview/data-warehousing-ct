## Surrogate keys in dimensions

Every dimension is built on a **surrogate key** — a warehouse-generated integer that identifies each row, kept deliberately separate from the source system's **natural (business) key**. In the Jabra model, `DIM_CUSTOMER` carries both:

| customer_key | customer_id | name | city | segment |
|--------------|-------------|------|------|---------|
| 1101 | C-4471 | Ana Ruiz | Madrid | B2C |

- `customer_key` — the **surrogate** primary key (`1101`), meaningless outside the warehouse, the thing the fact's foreign key points at.
- `customer_id` — the **natural** key (`C-4471`), the identifier the source operational system uses.

### Why a surrogate, not the natural key

- **Decoupled from the source** — if the source renumbers customers, or two merged systems both use `C-1`, the warehouse key is unaffected.
- **Fast, narrow joins** — a 4-byte integer beats a `varchar` business code on a fact of millions of rows.
- **A slot for the "Unknown" member** — reserve `0` (or `-1`) for *unknown / not-yet-arrived*, so a fact with a missing lookup gets a real key, never a NULL foreign key. This is how **late-arriving facts** are handled cleanly.

### The crucial one — surrogates enable history (SCD)

Here is the reason warehouses insist on it. When a dimension attribute **changes over time** — Ana moves from Madrid to Barcelona — a surrogate key lets you keep **both versions as separate rows**: same `customer_id` (`C-4471`), two different `customer_key`s (`1101` old, `1108` new). Each fact row points at whichever version was current *when the sale happened*, so history stays correct. A natural key alone can't do this — it identifies the customer, but not *which version* of them.

That technique is a **Slowly Changing Dimension**, and it gets the full type-by-type treatment in **module 06**. For now, the takeaway is one line:

> The surrogate key is the hinge SCD turns on. Give every dimension one, keep the natural key beside it, and reserve a key for the unknown member.
