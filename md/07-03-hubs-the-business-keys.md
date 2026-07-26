## Hubs — the business keys

A **hub** is the simplest table in the vault: the list of **unique business keys** for one entity, and nothing else. It answers exactly one question — *"which customers (or products, or orders) exist?"* — and stays deliberately bare.

### What's in a hub

`HubCustomer`, in full:

```
HubCustomer
  customer_hk     -- hash key (surrogate PK) = hash(customer_id)
  customer_id     -- the BUSINESS KEY from the source
  load_date       -- when this key first entered the vault
  record_source   -- where it first came from ('web','pos','erp')
```

One row per **distinct** `customer_id` ever seen. **No descriptive attributes** — no name, no city; those belong in a satellite. A hub is *identity only*.

### The stable spine

Business keys are the most **stable** thing in an enterprise — a customer number, an order number, a SKU rarely change, even as everything *about* them does. That stability makes the hub the **permanent anchor** every link and satellite attaches to. Build the hubs and the skeleton of the whole vault is fixed; the changeable parts hang off it.

### Hubs integrate the sources

Here's the quiet power. The **same** customer may appear in the web store, the POS, and the ERP — three systems, three records. Because the hub keys on the shared **business key** (`customer_id`), all three collapse to **one hub row**. The hub is where source systems are **integrated into a single version of each entity** — `record_source` still records where each was first seen, so you keep the lineage.

Jabra's vault has a hub per core entity: Customer, Product, Order, Invoice, Payment, Shipment, Promotion, Warehouse, Carrier.

> A hub is a bare list of unique business keys — identity only, no descriptions. It's the stable anchor of the vault and the point where many sources integrate into one entity.
