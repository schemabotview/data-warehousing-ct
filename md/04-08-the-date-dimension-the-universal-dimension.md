## The date dimension — the universal dimension

Almost every fact has a date, and almost every question has a time filter — "this quarter", "year over year", "weekends only". So one dimension appears in virtually every warehouse and joins to nearly every fact: **`DIM_DATE`**, the universal dimension.

### One row per calendar day, pre-built

`DIM_DATE`'s grain is **one row per calendar day**, populated **once, ahead of time**, for years past and future. Its surrogate key is a readable **`YYYYMMDD` integer** — `20260724` for 24 July 2026 — so the key is both a fast integer join *and* human-legible. Around it sits a wealth of attributes:

```
date_key   full_date   day_of_week  month_name  quarter  year
20260724   2026-07-24  Friday       July        3        2026
week_of_year  is_weekend  is_holiday  fiscal_year  fiscal_quarter
30            false       false       2026         3
```

### Why a table, not a SQL date function

You *could* call `MONTH()` or `EXTRACT()` at query time — so why store a table? Because a table holds what SQL **cannot compute**:

- **Business calendar** — `is_holiday` (Spanish national and regional), `fiscal_year`, `fiscal_quarter`. No date function knows your fiscal calendar or which days are holidays.
- **Consistent labels** — everyone gets the same `month_name` and `quarter` spelling, so reports agree.
- **Rich, ready attributes** — `is_weekend`, `week_of_year`, day/quarter names — filter and group without writing date logic in every query.
- **Fast integer joins** — the fact joins on a 4-byte `date_key`, not a date comparison.

### It role-plays, and it's conformed

`DIM_DATE` is the archetypal **conformed** dimension (shared by every fact) *and* the archetypal **role-playing** one (`order_date`, `ship_date`, `delivery_date` all point here). Build it richly, once, and the entire warehouse speaks the same calendar.

> Every warehouse builds a date dimension: one pre-computed row per day, a `YYYYMMDD` key, and every calendar and fiscal attribute a report could ask for.
