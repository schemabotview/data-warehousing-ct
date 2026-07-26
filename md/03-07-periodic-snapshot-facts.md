## Periodic snapshot facts

A transactional fact captures events *as they happen*. But some questions aren't about events — they're about **state**: how much stock is on hand, what's the account balance, how many subscribers do we have. For those you take a **periodic snapshot** — a photograph of a state at **regular intervals**.

### One row per entity per period

A periodic snapshot fact records the **state of something at the end of every period** — every day, week, or month — whether or not anything changed. The Jabra `FACT_INVENTORY_SNAPSHOT` is the classic case: its grain is **one row per product per warehouse per day**, capturing `on_hand_quantity`, `reserved_quantity`, `inventory_value`.

| snapshot_date | product | warehouse | on_hand_qty |
|---------------|---------|-----------|-------------|
| 01-Feb | Headset | Madrid | 1,270 |
| 02-Feb | Headset | Madrid | 1,180 |

You don't reconstruct this from transactions at query time — you **materialise** the level each period, so "stock on 1-Feb" is a single fast lookup.

### The measures are semi-additive

This is the catch that defines the type: snapshot measures are **semi-additive**. Add `on_hand_quantity` across all warehouses on one day — fine, total stock. Add the *same* product's stock across **every day** of the month — nonsense; you've counted the same units over and over. **Across time you average or take the last value, never sum.** (Snapshots are *the* reason the semi-additive class exists — see the measures section.)

### Transactional vs snapshot

- **Grain** — transactional is per *event*; snapshot is per *period*, at a **coarser** grain.
- **Density** — a snapshot has a row for every entity every period, even a quiet one (stock unchanged still gets a row).
- **Question** — transactional answers "what happened?"; snapshot answers "what was the level / balance?"

> Use a periodic snapshot when the business tracks a **level over time** — inventory, balances, headcount — and remember its measures don't sum across dates.
