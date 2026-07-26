## Star vs snowflake — the trade-off

Star and snowflake aren't rival philosophies — they're **two settings of one dial**: how normalized the dimensions are. Everything else follows from that single choice.

### The one axis, and what falls out of it

| Aspect | Star ★ | Snowflake ❄ |
|--------|--------|-------------|
| Dimensions | denormalized (flat) | normalized (multi-level) |
| Joins | fewer | more |
| Query speed | faster | slower |
| Query complexity | low | high |
| Foreign keys | fewer | more |
| Redundancy | more | less |
| Storage | more | less |
| Design | top-down | bottom-up |

Read it left to right and the pattern is clear: the star spends **storage** to buy **speed and simplicity**; the snowflake spends **speed and simplicity** to buy **storage and tidiness**.

### Which usually wins

For the overwhelming majority of warehouses, the **star wins**. The reasoning is economic:

- **Storage is cheap; query time and analyst effort are expensive.** The star spends the cheap resource to save the costly ones.
- **Simplicity compounds** — simpler SQL, easier BI-tool modelling, fewer ways to get a join wrong.
- The snowflake's headline saving — space — matters least, exactly where the star is weakest.

### When a snowflake earns its place

Reach for snowflaking only for a **specific** reason (next section expands this): a genuinely **huge dimension** with a large, highly repetitive sub-hierarchy; a sub-dimension **shared** across several dimensions (an outrigger, like Jabra's geography); or a **tool/governance** rule that requires normalized reference data.

> Same dial, opposite ends: star trades storage for speed and simplicity, snowflake the reverse. Default to the star; snowflake a dimension only when a concrete reason forces it.
