## Raw vault vs business vault

A mature Data Vault has **two layers**, and keeping them separate is what protects the vault's audit guarantee. The distinction is about **one thing**: has a business rule been applied yet?

### Raw vault — data exactly as it arrived

The **raw vault** holds source data **as-is** — loaded straight from the source with **no business rules, no cleansing, no interpretation**. If the source sent it, the raw vault stores it, verbatim, forever. This is the **system of record**: because nothing is ever transformed or overwritten, you can always prove **exactly what a source said and when**. The Jabra hubs, links, and satellites you've seen *are* the raw vault — the immutable integration layer.

### Business vault — where the rules live

The **business vault** is a **thin layer built on top** of the raw vault, holding data that results from **applying business logic**:

- **Computed / derived satellites** — a cleansed address, a standardized name, a calculated margin.
- **Derived links** — a relationship the source didn't state explicitly but the business infers.
- **Business-rule results** — de-duplication, classifications, KPIs.

It uses the *same* hub/link/satellite structures — it's just **downstream** of the raw layer and **derived**, not source-verbatim.

### Why split them

- **Auditability stays intact** — the raw vault is pristine; you never lose the original just because a rule was applied.
- **Rules can change without reloading** — a business rule is wrong or evolves? **Rebuild the business vault** from the untouched raw vault. The system of record never moves.
- **Interpretation is isolated** — everyone can see where raw fact ends and business opinion begins.

Keep the business vault **sparse** — add to it only what the raw vault can't answer directly. Then the information mart (next section) is built from **raw + business vault** together.

> The raw vault stores source data verbatim (the immutable system of record); the business vault, built on top, holds rule-derived data. Separating them keeps the audit trail pristine and lets business logic change without touching history.
