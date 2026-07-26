## Separation of compute & storage

The defining cloud-native breakthrough is **separating compute from storage**. It sounds like plumbing; it's what makes cloud warehouse economics and elasticity actually work.

### The old coupling, and the new split

Traditional MPP **coupled** the two: each node held both data (storage) and processing (compute). Scaling one forced scaling the other, and **resizing meant physically redistributing data** across nodes — slow and disruptive.

Cloud warehouses **decouple** them:

```
   compute clusters (stateless, on-demand)
        │        │        │
        └──── cloud object storage ────┘
              (S3 / GCS / Blob — cheap, durable)
```

Data lives once in **cheap object storage**; **stateless compute clusters** spin up on demand and read from it. Storage persists; compute is ephemeral.

### What that buys you

- **Scale independently** — throw more compute at a heavy query without touching storage, and grow storage without paying for compute.
- **Elastic, concurrent compute** — run **multiple independent clusters** ("virtual warehouses") over the **same** data. Isolate workloads — ETL on one cluster, BI on another — with **no contention**.
- **Pay separately** — cheap storage always on; compute billed only while running (even auto-suspending when idle).
- **Instant resize** — add/remove compute with **no data movement**, because the data never lived on the compute nodes.

### In the platforms

Snowflake popularised the model (virtual warehouses over shared storage); **BigQuery** goes further to **serverless** — no clusters to manage at all, compute allocated per query. This decoupling is why you can give the ETL job and the dashboards their own compute over one copy of the Jabra warehouse.

> Separating compute from storage puts data in cheap object storage and runs stateless compute on demand — so you scale each independently, run isolated concurrent clusters over one dataset, pay only for compute you use, and resize instantly.
