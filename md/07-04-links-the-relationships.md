## Links — the relationships

If hubs are the entities, **links** are the **relationships and transactions between them**. A link records that two or more hubs are connected — and, like a hub, it holds **keys only**, no descriptive data.

### What's in a link

`LinkOrderLine` connects an order to a product — which is exactly the *sale-line* grain:

```
LinkOrderLine
  order_line_hk   -- hash key (PK) = hash(order_hk + product_hk)
  order_hk        -- FK → HubOrder
  product_hk      -- FK → HubProduct
  load_date, record_source
```

`LinkOrderCustomer` connects an order to the customer who placed it (`order_hk` + `customer_hk`). A link is just the **hash keys of the hubs it ties together**, plus the standard metadata — no attributes, no measures.

### The golden rule — links are always many-to-many

This is the design decision that gives Data Vault its agility: **model every relationship as many-to-many**, even if today it looks one-to-many. Because a link imposes no cardinality, the relationship can **change** — one order gaining many shipments, a product appearing on many orders — **without ever restructuring the model**. In a star, a cardinality change can mean rebuilding a fact; in a vault, the link already allows it.

### Where the relationship's data goes

A link carries no descriptive columns — so the **measures and attributes of the relationship live in a satellite on the link**. `LinkOrderLine` has no numbers itself; its `SatSalesMeasures` satellite holds `quantity`, `unit_price`, `line_total`. (Satellites are the next section.)

A link is the vault's analogue of a fact's key-set, or a bridge table: it captures *that* things relate; the satellite captures *the details*.

> A link records a relationship between hubs using their hash keys only, and is always modelled many-to-many — so relationships can evolve without redesign. Its measures ride in a satellite hung off it.
