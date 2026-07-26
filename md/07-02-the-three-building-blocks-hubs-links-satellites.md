## The three building blocks — hubs, links, satellites

Data Vault models *everything* with exactly **three** table types. Each captures **one** of the three concerns a star fuses together, and keeping them physically separate is the whole source of the vault's agility.

### One concern each

- **Hub — identity.** The list of unique **business keys** for an entity. "This customer exists." Just the key, nothing descriptive.
- **Link — relationship.** A connection or transaction **between hubs**. "This customer placed this order." Just the keys of the things related.
- **Satellite — context.** The **descriptive attributes and measures**, with **full history**, hanging off a hub or a link. "This customer's name and city, as loaded on this date."

### How they connect

Hubs are the anchors; links tie hubs together; satellites describe hubs and links. The Jabra vault in miniature:

```
   SatCustomer                      SatOrder
        │                              │
   HubCustomer ── LinkOrderCustomer ── HubOrder ── LinkOrderLine ── HubProduct
                                                        │
                                                 SatSalesMeasures
```

`HubCustomer` and `HubOrder` are joined by `LinkOrderCustomer`; each hub is described by its satellite; the order-line measures live in a satellite on the link.

### Why split it three ways

Because each concern now **changes independently, without touching the others**:

- A **new descriptive attribute** → add a column to a satellite, or a new satellite. Hubs and links unchanged.
- A **new relationship** → add a link. Hubs and satellites unchanged.
- A **new source or entity** → add a hub (+ its satellite). Everything else unchanged.

That "add, never restructure" property is exactly the agility section 01 promised — and it falls straight out of the three-way separation.

> Three tables, three jobs: hubs hold keys, links hold relationships, satellites hold context and history. Separating them is what lets a Data Vault grow by addition instead of redesign.
