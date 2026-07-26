## Building the dimensions with surrogate keys

The bill's **attribute** fields become the dimension tables. Group the A fields **by the entity they describe**, and each group is a dimension — fitted with a surrogate key, its natural key kept, and an SCD policy chosen per column.

### Group the attributes into dimensions

| Bill attributes | → Dimension |
|-----------------|-------------|
| customer id, name, ship/bill address | `DIM_CUSTOMER` |
| product id, name, price, line, category | `DIM_PRODUCT` |
| order date | `DIM_DATE` |
| channel | `DIM_CHANNEL` |
| promo code | `DIM_PROMOTION` |

### Fit each dimension out

For each one, apply the module-04 checklist:

- **Add a surrogate key** — `customer_key`, `product_key`, … — the integer the fact will point at.
- **Keep the natural key as an attribute** — `customer_id`, `product_id` — for source traceability.
- **Choose SCD per attribute** — customer `address` needs history → **Type 2**; a corrected `name` → Type 1 (module 06).
- **Reuse the conformed version** — `DIM_DATE`, `DIM_CUSTOMER` already exist enterprise-wide; point at them rather than rebuild.

`DIM_CUSTOMER`, built:

```
DIM_CUSTOMER
  customer_key    PK|SK   -- surrogate
  customer_id     NK      -- from the bill
  name, segment, city, province, region, country
  effective_date, expiry_date, is_current   -- SCD-2
```

### Why the surrogate matters here

The bill only knows the **natural** key (`C-4471`). The dimension mints a **surrogate** (`1101`) so the fact can join on a fast integer *and* carry history — Ana's address change becomes a second row with a new `customer_key`, and old order lines still point at the old version (module 06). The surrogate is what lets one bill's customer become a *versioned* dimension row.

> Group the bill's attributes by entity into dimension tables, each with a surrogate key, its natural key retained, and a per-attribute SCD policy — reusing conformed dimensions where they already exist.
