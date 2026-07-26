## Data distribution — distribution & partition keys

On an MPP warehouse, **how the fact's rows are spread across the nodes** decides how fast a query runs. Two levers control it: the **distribution key** (which node a row lives on) and the **partition key** (which segment within a node). They're different axes, and both matter.

### Distribution key — which node

The **distribution key** is the column used to assign each row to a compute node (usually by hashing it). Choose it for two goals:

- **Even spread, no skew** — pick a **high-cardinality, evenly-distributed** column so every node gets a roughly equal share. A skewed key overloads one node and the whole query waits on it.
- **Co-located joins** — if a fact and a big dimension are distributed on the **same** key, matching rows land on the **same node**, so the join runs **locally** with no network **shuffle**. Distribute `FACT_SALES` and a large `DIM_PRODUCT` on `product_key` and their join stays node-local.

Small dimensions don't need co-location — they're **broadcast** (replicated to every node) so any fact row can join them locally.

### Partition key — which segment

The **partition key** splits a table into **segments** by a column — very often **date**. A query filtered on that column then reads **only the relevant partitions** and skips the rest (**partition pruning**, section 09). Partitioning `FACT_SALES` by `order_date` means "sales in 2026" touches one year's partitions, not ten.

### Two axes, one goal

- **Distribution** = *which node* a row is on → balance work, localise joins.
- **Partition** = *which segment* → scan less by pruning.

Redshift and Synapse make distribution **explicit** (`DISTKEY`); Snowflake and BigQuery **automate** distribution but let you choose **partition/cluster** columns. Get these wrong — a skewed distribution or no partitioning — and even MPP crawls under shuffles and full scans.

> Distribution key decides which node a row lives on (balance it, and align fact + big-dim keys for local joins); partition key splits a table into segments (usually by date) so filters prune to just the relevant ones.
