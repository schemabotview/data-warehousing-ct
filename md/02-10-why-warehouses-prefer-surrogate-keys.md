## Why warehouses prefer surrogate keys

The last section showed *one* reason a surrogate helps. In a warehouse the case is overwhelming — nearly every dimension is keyed on a surrogate, with the natural key demoted to an attribute. Here's the full argument.

### The five reasons

- **Stability** — the surrogate stays fixed even when source data changes. Emails change, product codes get reissued, companies merge and renumber — the warehouse key doesn't budge, so nothing downstream breaks.
- **Performance** — a small integer makes **smaller indexes and faster joins**. A fact table with millions of rows joins to its dimensions on narrow integer keys instead of long text natural keys; over billions of key comparisons, that's a large, real saving.
- **Flexibility (multi-source integration)** — when you merge data from many systems with **clashing IDs** (two source systems each with a "customer 1"), the surrogate gives every real entity one clean, conflict-free key. It insulates the warehouse from the source's key design entirely.
- **Simplicity** — simple sequential integers are efficient to store, generate, and reason about; a fact row's foreign keys are all uniform `int`s.
- **Enables SCD Type 2** — this is the decisive one. To keep **history**, a dimension needs *multiple rows for the same real-world entity* (Anita before and after her address change). A natural key can't do that — it must be unique. The surrogate makes each historical version its own row with its own key, all sharing one natural key.

### The costs — and why they're acceptable here

| Drawback | Why the warehouse accepts it |
|----------|------------------------------|
| No business meaning (not human-readable) | The natural key is kept as an attribute for tracing |
| Extra column + a lookup to map back to source | The **ETL** does this lookup once, at load (Module 09) |
| Must be system-generated | The load pipeline owns key generation |
| Technically violates 3NF | The warehouse is denormalized on purpose anyway |

Every drawback is either paid once by the pipeline or irrelevant given the warehouse is already denormalized for reads.

### How it looks

```sql
CREATE TABLE DimCustomer (
  CustomerSK  int IDENTITY(1,1) PRIMARY KEY,  -- surrogate: the warehouse key
  CustomerNK  varchar(50),                    -- natural key, kept as an attribute
  FirstName   varchar(50),
  LastName    varchar(50)
);

CREATE TABLE FactSales (
  CustomerSK  int,          -- FK on the surrogate — insulated from the source
  SalesAmount money,
  FOREIGN KEY (CustomerSK) REFERENCES DimCustomer(CustomerSK)
);
```

The fact points at `CustomerSK`, never at the email or source ID. **That's the pattern the whole star schema is built on** — surrogate-keyed dimensions, facts joining to them on integers. This closes the Normalization & Keys module: we normalize to protect data, denormalize dimensions for speed, and key them on surrogates for stability, performance, and history. Next module: the **fact table** those keys point back to.
