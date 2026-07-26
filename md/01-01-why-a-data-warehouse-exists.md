## Why a data warehouse exists

Your operational databases are built to answer one kind of question, fast: *what is true right now* — for this order, this customer, this account. They are tuned for tiny, constant reads and writes, not for scanning years of history across millions of rows.

But the business needs the other kind of question: *what happened across everything, over time* — total sales by region last quarter, which products sell together, how a customer's value changed year over year. Run a question like that on the live operational system and you slow the business down — and even then you still can't reach the data trapped in all the other systems beside it.

A **data warehouse** exists to solve exactly this. It is a separate, central store that collects data from every source system into a single location, cleans and organizes it once, and keeps its history — so those heavy analytical questions are easy to **access, analyze, and report on**, they run fast, and they never touch production.

### The problems it solves

- **One machine / one system can't scale the analysis** — heavy scans over history don't fit the operational database's job.
- **Data is siloed** — the numbers you need live in many separate systems (CRM, ERP, files, apps) that don't talk to each other.
- **Operational systems overwrite** — they keep only the current value, so history for trend analysis is lost.
- **Analytics competes with production** — running big reports on the live system degrades the customer-facing app.

### Why it pays off — the benefits

- **Better decision-making** — one consistent, historical view to reason over, instead of scattered snapshots.
- **Increased efficiency** — analysts self-serve fast queries instead of waiting on operational teams or fragile exports.
- **Enhanced data quality** — data is cleaned, conformed, and reconciled once, on the way in.
- **Better customer insight** — history and cross-system integration reveal behavior a single app never could.

The rest of this module builds out *what* a warehouse is, how it differs from an operational database (OLTP vs OLAP) and a data lake, its components, and where it sits in the modeling journey.
