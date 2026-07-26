## Satellites — descriptive history

Hubs and links hold only keys. **All the descriptive data** — every attribute, every measure — lives in **satellites**. And a satellite doesn't just store the current value; it stores **every value over time**. This is where the vault's history lives.

### What's in a satellite

A satellite hangs off **one** parent — a hub or a link. `SatCustomer` describes `HubCustomer`:

```
SatCustomer
  customer_hk    -- FK → parent hub
  load_date      -- part of the PK
  hash_diff      -- hash of the attributes (change detection)
  record_source
  name, email, segment, city, province, region, country, postal_code
```

The primary key is **`(customer_hk, load_date)`** — parent plus load timestamp. That composite key is the whole trick: **many rows per parent, one per point in time.**

### History is built in

Because the key includes `load_date`, tracking a change is just an **insert**. Ana's city changes from Madrid to Barcelona? You insert a **new** `SatCustomer` row with today's `load_date` and `city = Barcelona`; the old Madrid row **stays**. Nothing is overwritten. The satellite *is* an SCD-2 timeline, native — no effective/expiry bolt-on needed, because `load_date` sequencing gives you the versions for free.

### Two kinds of satellite

- **On a hub → descriptive context.** `SatCustomer`, `SatProduct` — names, categories, prices; the stuff that becomes dimension attributes.
- **On a link → transaction measures.** `SatSalesMeasures` on `LinkOrderLine` — `quantity`, `line_total`; the stuff that becomes fact measures.

### Split by source or rate of change

One hub can carry **several** satellites — commonly split by **source system** (one satellite per source, loaded independently) or by **rate of change** (fast-changing attributes in one satellite, slow in another). Splitting keeps loads **parallel** and avoids rewriting stable data when only a volatile field moves.

> Satellites hold all descriptive attributes and measures, keyed by parent + `load_date` — so every change is a new row and full history is automatic. They're native SCD-2, split by source or volatility for efficient parallel loads.
