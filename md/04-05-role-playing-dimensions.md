## Role-playing dimensions

One physical dimension table can appear **several times in the same fact, each time in a different role**. That's a **role-playing dimension** — one table, many jobs.

### The date dimension, three times over

`FACT_SHIPMENT` records two dates: when a parcel shipped, and when it was delivered. `FACT_SALES` records the order date. All three are dates — so all three point at the **same** `DIM_DATE`:

```
order_date_key ───┐
ship_date_key ────┼── DIM_DATE  (one table, three roles)
delivery_date_key ┘
```

You do **not** build `DIM_ORDER_DATE`, `DIM_SHIP_DATE`, `DIM_DELIVERY_DATE` — that would triplicate the same calendar and let the copies drift. You build **one** `DIM_DATE` and join it as many times as there are date columns, each join playing a different **role**.

### Making the roles readable

Each join needs its own alias so a query reads naturally and columns don't collide:

```sql
SELECT   o.month_name  AS order_month,
         d.month_name  AS delivery_month
FROM     fact_shipment f
JOIN     dim_date o ON f.ship_date_key     = o.date_key
JOIN     dim_date d ON f.delivery_date_key = d.date_key;
```

Some teams expose each role as a **view** (`vw_order_date`, `vw_ship_date`) over the one physical table, so BI tools show tidy, role-named fields.

### Beyond dates

Dates are the common case, but any reused dimension role-plays. An **employee** dimension can appear as both `seller_key` and `buyer_key` on the same fact; a **geography** dimension as both `origin` and `destination` on a shipment.

> One physical table, joined once per role, aliased per use. Role-playing keeps a single conformed dimension while letting a fact reference it from several angles.
