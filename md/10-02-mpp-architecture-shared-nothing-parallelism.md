## MPP architecture — shared-nothing parallelism

The engine under every cloud warehouse is **MPP — massively parallel processing**. Instead of one big machine chewing through a query row by row, MPP spreads the work across **many nodes running in parallel**.

### Shared-nothing

The classic MPP design is **shared-nothing**: each compute node owns a **slice of the data** and has its **own CPU, memory, and disk**. Nodes don't contend for a shared resource, so there's no central bottleneck — add nodes and you add capacity almost linearly.

```
        ┌── node 1 ── slice of FACT_SALES
query ──┼── node 2 ── slice of FACT_SALES   → combine → result
        └── node 3 ── slice of FACT_SALES
```

### How a query runs

A **leader/coordinator** node plans the query and splits it into fragments; each **compute node** runs its fragment against its own slice; the partial results are combined into the final answer. Scanning and aggregating a fact table is **embarrassingly parallel** — split a billion-row fact across 100 nodes and each scans ten million rows at once, a near-100× speedup.

### Why it fits analytics (and not OLTP)

- **Analytics** = scan and aggregate huge row sets → parallelism wins big.
- **OLTP** = fetch/update single rows → a single node is fine; MPP's coordination overhead wouldn't pay off.

This is exactly the workload split from module 01 (OLTP vs OLAP), now realised in hardware. **Scale out** — add nodes — is how you go faster, versus the old world's **scale up** (a bigger box).

> MPP runs a query across many shared-nothing nodes, each owning a data slice with its own CPU/memory/disk. Scanning a huge fact is embarrassingly parallel — the engine that makes star joins over billions of rows fast.
