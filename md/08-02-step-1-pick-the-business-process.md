## Step 1 — pick the business process

The first step is to choose **one business process** to model. A business process is a real **operational activity that produces measurable events** — *taking a sales order*, *shipping a parcel*, *settling a payment*, *counting stock*. It is emphatically **not** a department, a report, or a dashboard; it's the *activity* those things sit on top of.

### One process → one fact table

Each business process becomes **one fact table** — one star. That's why you pick *one* at a time: you build the warehouse **process by process**, and the shared conformed dimensions stitch the stars into a galaxy (module 05). Trying to model "everything" at once is how designs collapse; modeling one clean process is how they succeed.

### How to choose which one

Pick by **business value × data readiness**:

- **Value** — start with the process that answers the organisation's most pressing questions. For a retailer that's almost always **sales**.
- **Readiness** — favour a process whose source data is available, clean, and well understood, so you get a win on the board early.

The classic first pick is **sales orders** — high value, familiar, and rich in dimensions.

### Our worked process

For Jabra Spain we'll model **"a customer places a sales order"**. That process throws off an event every time a product is bought, and those events become the rows of what will be `FACT_SALES`. Fixing the process now sets up everything downstream — the events we capture, and, next, the grain at which we capture them.

> Step 1 picks a single operational process — sales orders — chosen for business value and clean data. One process becomes one fact table; the warehouse grows one star at a time.
