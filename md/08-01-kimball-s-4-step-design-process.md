## Kimball's 4-step design process

You've met the parts — facts, dimensions, grain, keys, SCD. This module puts them together: faced with a blank page and a business, **how do you actually design the model?** Kimball's answer is a disciplined, repeatable recipe — **four steps, always in this order**:

1. **Pick the business process** — the operational activity you'll model.
2. **Declare the grain** — what one fact row represents.
3. **Identify the dimensions** — how you describe and slice it.
4. **Identify the facts** — the measures you'll aggregate.

### The order is not optional

Each step **constrains the next**, so they only work in sequence. You can't list dimensions or facts until the grain is fixed — the grain is what tells you which attributes and measures are even *valid* at that level of detail. Skip or reorder a step and the model wobbles: mixed grain, orphaned measures, dimensions that don't fit.

### Design from the business, not the source

A subtle but central point: you model a **business process the organisation cares about measuring** — *sales*, *shipments* — not a copy of the source system's tables. The source is where the *data* comes from; the *process* is what you design around. That's why step 1 is a business question, not a database question.

### What this module does

We'll walk each of the four steps, then apply all four to a **real source document — a customer bill** — classifying its fields, building the dimensions and the fact, assembling the finished **star**, and finally reading it as **DBML**. It's the whole course made practical: modules 03–06 turned into a checklist you can run on any process.

> Kimball's four steps — process, grain, dimensions, facts — turn modeling from art into a repeatable procedure. Run them in order, from the business process down, and a sound star falls out.
