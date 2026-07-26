## From Data Vault to star — the information mart

The vault is superb for integrating and preserving data — and terrible to query. So the last step turns it into something people can actually use: an **information mart**, a **star schema built on top of the vault** for consumption. This is where the two halves of the course meet.

### The mapping is mechanical

Vault structures map onto star structures almost one-to-one:

- **Hub + its satellite(s) → a Dimension.** `HubCustomer` + `SatCustomer` become `DIM_CUSTOMER`. The satellite's `load_date` history *is* your SCD-2 — you get versioned dimensions for free, no bolt-on needed.
- **Link + its measure satellite → a Fact.** `LinkOrderLine` + `SatSalesMeasures` become `FACT_SALES`. The link's grain (product-on-order) is exactly the fact's grain (order line).
- **Reference tables → small dimensions.** `RefDate` → `DIM_DATE`, `RefChannel` → `DIM_CHANNEL`.

Collapse the vault's many small tables back into a few wide star tables, and you have the Jabra `star-schema` from modules 03–06 — the **information mart** the raw vault was built to feed.

### Virtual or materialized

The mart is often just a set of **views** over the vault — a **virtual mart**, always live, nothing duplicated. When query volume demands it, you **materialize** it instead (a physical rebuild, refreshed by ETL). Either way the vault stays the source of truth.

### Why this is the payoff

Because the vault kept **full history** and stayed **auditable**, you can:

- **Rebuild** the mart any time — reshape a dimension, add a fact — without going back to the sources.
- Generate **point-in-time** versions of the mart — "the star as it looked last quarter."
- Serve **many** marts (Sales, Finance, Marketing) from **one** integrated vault.

> The information mart is a star built over the vault: hubs+satellites become dimensions, links+measure-satellites become facts, all as views or a materialized rebuild. The vault integrates and remembers; the mart presents — the promise of section 01, realized.
