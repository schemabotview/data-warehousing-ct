## Reading the model as DBML

A finished model needs to be **written down** — precisely, shareably, and in something a tool can render. The Jabra warehouse is captured as **DBML** (Database Markup Language): a concise text notation for tables and their relationships, which renders to an ERD at dbdiagram.io.

### The syntax, in one look

```dbml
Table FactSales {
  sales_key       integer [primary key, note: 'surrogate key']
  order_date_key  integer [not null]
  customer_key    integer [not null]
  product_key     integer [not null]
  order_id        integer [note: 'degenerate dimension']
  quantity        integer
  line_total      decimal(12,2)
}

Table DimCustomer {
  customer_key    integer [primary key]
  customer_id     integer [not null, note: 'natural key']
  name            varchar
  region          varchar
}

Ref: FactSales.customer_key > DimCustomer.customer_key
```

### How to read it

- **`Table … { }`** — one table; each line is a **column**, its type, and optional tags.
- **`[primary key]`** — marks the surrogate PK; **`[not null]`** the required FKs.
- **`[note: '…']`** — documents intent: which columns are degenerate dimensions, natural keys, SCD-2.
- **`Ref: FactSales.customer_key > DimCustomer.customer_key`** — one **edge of the star**: the fact's FK (`>` = many-to-one) to the dimension's PK. Read all the `Ref:` lines and you've read the star's shape.

### Why DBML

- **Version-controllable & reviewable** — the model is text, so it diffs and reviews like code.
- **The single source of the physical model** — one file defines tables, keys, and relationships.
- **Renders an ERD** — paste into dbdiagram.io for the picture, no manual drawing.

The full model lives in `jabra-spain-dw-model.dbml` — the star from this whole module, made concrete and shareable.

> DBML captures the finished model as reviewable text: `Table` blocks are tables, `[primary key]` the surrogate PKs, and each `Ref:` an FK→PK edge of the star — a version-controlled source of truth that renders straight to an ERD.
