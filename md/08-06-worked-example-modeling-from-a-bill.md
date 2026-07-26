## Worked example — modeling from a bill

Now run all four steps on a **real source document** — a Jabra customer **bill** (an order invoice). A bill is perfect input: it's exactly what the business hands a customer, so it contains every field the sales process actually produces.

### What's on the bill

```
Order #4471   Date: 2026-04-12   Channel: Web   Promo: SPRING10
Customer: Ana Ruiz (C-4471)   Ship/Bill: Calle Mayor 3, Barcelona
────────────────────────────────────────────────────────────
Product              Price   Qty   Subtotal   Tax
Evolve2 65 (P-88)    600.00    2    1,200.00   252.00
Elite 8 (P-91)       300.00    1      300.00    63.00
```

### The technique — classify each field: Attribute or Measure

Walk every field and tag it **A** (attribute — descriptive context you slice by) or **M** (measure — a number you sum):

| Field | A/M | | Field | A/M |
|-------|-----|--|-------|-----|
| Order id | A | | Customer id / name | A |
| Order date | A | | Ship / bill address | A |
| Product id / name | A | | Channel | A |
| Price | A | | Promo code | A |
| **Quantity** | **M** | | **Subtotal** (line_total) | **M** |
| **Tax** | **M** | | | |

### Why this single pass *is* steps 3 and 4

The A/M split does the design work:

- **Attributes → dimensions.** Group the A fields by entity — customer fields → `DIM_CUSTOMER`, product fields → `DIM_PRODUCT`, and so on (section 07 builds these).
- **Measures → the fact.** The M fields are the fact's measures (section 08 builds it).

### The trap the test avoids

Note **Price is an A**, even though it's a number — because you **don't sum prices** (it's a rate). The test is *"do you aggregate it?"*, **not** *"is it numeric?"* — the same "sum it or slice by it" test from module 04. Get that right and the fact stays additive.

> Classify every field on a real bill as Attribute or Measure: attributes become dimensions, measures become the fact. One A/M pass turns a source document into a star — and flags rates like price as attributes, not measures.
