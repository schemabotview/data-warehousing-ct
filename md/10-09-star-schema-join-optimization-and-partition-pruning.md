## Star-schema join optimization & partition pruning

Here's the payoff of choosing a **star** (module 05): the MPP optimizer has specific tricks for exactly that shape, so a well-designed star runs fast **because** it's a star. Three optimizations do the heavy lifting.

### Partition pruning — scan only relevant segments

The query's filters decide which **partitions** are even touched. `WHERE year = 2026` reads only 2026's partitions and **skips the rest**; **zone maps** (section 07) then skip blocks within them. A ten-year fact answers a one-year question by scanning roughly a tenth of the data — often far less.

### Star-join optimization — filter the fact by the dimensions

The optimizer uses the **small dimensions to filter the huge fact** before scanning it fully. For `WHERE product_line = 'Headsets'`:

1. Scan the *small* `DIM_PRODUCT`, collect the matching `product_key`s.
2. Build a **bloom filter** from them and push it down to the fact scan.
3. Read only fact rows whose `product_key` is in the set — skip the rest.

This **semi-join / bloom-filter pushdown** avoids reading the whole fact just to throw most of it away.

### Broadcast & co-located joins

Small dimensions are **broadcast** to every node so the fact joins them **locally**; a fact and a big dimension sharing a **distribution key** join without a **shuffle** (section 05). Either way, no cross-network data movement.

### Why star beats snowflake even harder here

Each of these — pruning, bloom-filter pushdown, broadcast — works best with **few, wide, denormalized dimensions** one hop from the fact. A snowflake's extra join levels give the optimizer more chances to shuffle and less to prune. So on MPP the star's advantage **widens** (module 05's modern tilt).

> A star runs fast on MPP by design: partition pruning scans only relevant segments, star-join optimization uses small dims (via bloom filters) to filter the huge fact, and broadcast/co-located joins avoid shuffles. The denormalized star is what makes these possible.
