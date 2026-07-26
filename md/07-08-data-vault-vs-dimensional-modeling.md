## Data Vault vs dimensional modeling

Data Vault and the star schema look like opposites — one exploded into dozens of tiny tables, the other collapsed into a few wide ones. But they're **not rivals**. They're **optimized for different jobs**, and the right architecture uses **both**.

### Different jobs, opposite shapes

- **Dimensional (star)** is optimized for **querying and reporting**. Denormalized, few tables, fast joins, labels business users understand. Its weaknesses: **rigid** to structural change, and history is **bolted on** via SCD.
- **Data Vault** is optimized for **integration and loading**. Highly normalized into many small tables, **agile** to change, history and audit **native**, **parallel-loadable**. Its weakness: **not** something a business user can query — far too many joins.

### Side by side

| | Dimensional (star) | Data Vault |
|--|--------------------|------------|
| Optimized for | query & reporting | integration & loading |
| Structure | denormalized, few tables | normalized, many tables |
| History | SCD (bolted on) | native (satellites) |
| Agility to change | low | high |
| Query performance | fast, simple | slow, many joins |
| Audience | analysts, BI tools | ETL / engineering |
| Auditability | limited | full (source + time) |

### The resolution — layer them

You don't pick one. You **stack** them:

```
sources → DATA VAULT (raw integration, system of record)
                     → STAR information marts (query layer) → BI
```

The vault is the **integration layer** — it absorbs every source, keeps full history, and stays auditable and agile. The star is the **presentation layer** — built on top for fast, friendly analysis. Each does what it's best at, and the star's rigidity stops mattering because you can always **rebuild** it from the vault.

> Data Vault and dimensional modeling aren't competitors — one is built to *load and integrate*, the other to *query*. The mature pattern layers them: a Data Vault system-of-record feeding star information marts.
