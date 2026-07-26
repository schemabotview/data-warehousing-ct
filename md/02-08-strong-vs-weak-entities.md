## Strong vs weak entities

Foreign keys reveal that some tables **depend** on others to even make sense. That dependency splits real-world things into two kinds of entity.

### Strong entity — stands on its own

A **strong entity** has an **independent existence** and its own identifying key. A `CUSTOMER`, a `PRODUCT`, a `DEPARTMENT` — each is meaningful by itself and is identified by its own primary key (`customer_id`, `product_id`). Delete everything around it and the entity still makes sense.

### Weak entity — can't exist alone

A **weak entity** depends on another entity for its existence and often can't be uniquely identified without it. An **order line** has no meaning without its **order**; a **payment installment** has no meaning without its **loan**. A weak entity:

- **Depends on a parent** (its *owner*) — remove the parent and the child should go too.
- Usually lacks a key of its own — it's identified by the **parent's key plus a local discriminator** (a *partial key*), forming a composite key: `(order_id, line_no)`.
- Is linked to its owner through a **foreign key** — the FK is part of what identifies it.

```
ORDER  (order_id PK, order_date, customer_id FK)        -- strong
  └── ORDER_LINE  (order_id FK, line_no, product_id, qty) -- weak
      PK = (order_id, line_no)
```

The order line borrows `order_id` from its parent and adds `line_no` to tell its siblings apart. That's the signature of a weak entity: **partial key + parent key = identity**.

### Why it matters for modeling

- It tells you where a **composite key** and a **mandatory foreign key** belong.
- It signals **lifecycle coupling** — deleting a parent should cascade to its weak children (an order's lines die with the order).

### In the warehouse

This distinction softens once you're dimensional. A weak entity's parent relationship is frequently **collapsed** into the fact or a dimension rather than kept as a separate dependent table — order-line detail becomes the **grain of the sales fact**, with `order_id` riding along as a **degenerate dimension** (an identifier kept on the fact with no dimension table of its own). So the modeling instinct carries over even when the literal weak-entity table does not — a thread we pick up in the fact-table module.
