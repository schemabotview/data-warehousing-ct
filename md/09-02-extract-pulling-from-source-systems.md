## Extract — pulling from source systems

The pipeline begins with **extract**: pulling data **out of the source systems** where the business actually runs. For Jabra that's the **web store**, the **POS**, the **ERP**, and the **payment gateway** — each a separate system with its own format and schema.

### The challenges of extraction

- **Heterogeneous sources** — relational databases, application APIs, flat files, event logs, SaaS exports. Each speaks differently; extract must handle each.
- **Don't overload the source** — these are **live operational systems**; a heavy extract can slow the business. Pull during off-hours, throttle, or read from a **replica** rather than the primary.
- **Read-only, minimal footprint** — extraction never writes back to the source; it takes a faithful copy and leaves the system untouched.

### Full vs incremental extract

Two modes, mirroring the load choice later:

- **Full extract** — grab the entire dataset every run. Simple; fine for small or slowly-changing sources.
- **Incremental extract** — pull only what **changed since last run**, using a `last_modified` timestamp or **change data capture** (section 06). Essential for large, busy sources — you can't re-pull millions of orders nightly.

### Preserve source fidelity

Extract lands the data **as-is** — unchanged, uninterpreted — into the staging area (next section). Keeping a faithful raw copy matters for **auditability** (prove what the source said) and **reprocessing** (re-run transforms without re-hitting the source). Cleaning and conforming come *later*, in transform — extract's only job is to get the raw data out, safely and completely.

> Extract pulls raw data from many heterogeneous source systems — read-only, low-impact, full or incremental — and lands it faithfully in staging, leaving interpretation for the transform stage.
