## Step 4 — identify the facts

The final step names the **facts** — the numeric **measures** you'll aggregate, each one true **at the grain**. For the order line, the measures are:

`quantity`, `unit_price`, `discount_amount`, `tax_amount`, `line_total`.

### Test every candidate measure

For each number, ask two things:

- **Is it numeric and something you'd aggregate?** `quantity`, `line_total` — yes, you `SUM` them across dimensions.
- **Is it true at *this* grain?** A per-line quantity, yes. An **order-level** shipping charge — *no*: it belongs to the order, not the line, so it can't sit at order-line grain (it would double-count across the order's lines). Grain mismatches are the classic step-4 trap.

### Mind additivity

Recall module 03. Most measures here are **additive** (`quantity`, `discount_amount`, `tax_amount`, `line_total`) — the ideal. But `unit_price` is a **rate**: keep it if useful, but know it's **non-additive** — you never `SUM` prices; you'd derive an average price *after* aggregating. Store additive **amounts**, and compute ratios last.

### Don't forget the degenerate dimensions

Two fields left over from the bill are neither measures nor worth a dimension table: `order_id` and `order_status`. They're **degenerate dimensions** (modules 03–04) — operational identifiers that ride **on the fact** for grouping and audit.

### Step 4 completes the fact's columns

The four steps now hand you the whole fact row:

- **Foreign keys** (from step 3's dimensions) — `order_date_key`, `customer_key`, `product_key`, `channel_key`, `promotion_key`.
- **Measures** (step 4) — `quantity` … `line_total`.
- **Degenerate dimensions** — `order_id`, `order_status`.

> Step 4 picks the measures true at the grain — additive amounts preferred, rates flagged non-additive, grain mismatches rejected — plus any degenerate dimensions. Steps 3 + 4 together are the fact's columns.
