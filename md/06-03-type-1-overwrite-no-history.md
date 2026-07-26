## Type 1 — overwrite (no history)

**Type 1** is the everyday strategy: when a value changes, **overwrite it in place**. The new value replaces the old, the old value is gone, and **no history is kept**.

### One row, updated

Ana moves from Madrid to Barcelona. Her single `DIM_CUSTOMER` row simply changes:

```
before:  1101 | C-4471 | Ana Ruiz | Madrid
after:   1101 | C-4471 | Ana Ruiz | Barcelona
```

Same `customer_key` (`1101`), same row — just a new `city`. Mechanically it's an **UPSERT**: update the row if it exists, insert it if it's new. It's the simplest type to build and the most common for attributes that don't need history.

### What it does to the facts

Because every fact points at `customer_key = 1101`, and that one row now says Barcelona, **all of Ana's past sales instantly roll up under Barcelona** — including the ones she made while in Madrid. Type 1 **rewrites the past**: the world is reported entirely **as it is now**. There is no way to ask "what was her city at the time of that sale" — that information is overwritten and lost.

### When Type 1 is right

- **You don't need history** — the attribute isn't something you analyze over time (a formatting tweak, a phone number).
- **Corrections** — fixing a misspelled name or a bad code. Here overwrite is *exactly* what you want: you never wish to preserve the wrong old value.
- **"As-is" is the only question** — you always want the current value applied to all history.

### The limitation

Type 1 is lossy by design. The moment the business asks "*revenue by region as it stood back then*", Type 1 can't answer — it only knows the present. When history matters, you reach for Type 2.

> Type 1 overwrites: one row, new value, no history — simple and perfect for corrections and non-historical attributes, but it rewrites the past into the present.
