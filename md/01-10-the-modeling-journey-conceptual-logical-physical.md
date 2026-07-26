## The modeling journey — conceptual, logical, physical

Designing the data itself moves through three levels of abstraction, from a business idea to running tables. Each level adds detail, and each has its own audience.

### Conceptual model

The **big-picture** view: the key business entities and their relationships, no technology. "A Customer places Orders; an Order contains Products." Boxes and lines a business stakeholder can validate. No columns, keys, or types.

### Logical model

Add **structure, still platform-independent**: entities become tables, attributes become columns, relationships get keys (primary and foreign), and the design is normalized (for OLTP) or dimensioned into facts and dimensions (for a warehouse). Data types are generic. This is where the *modeling* decisions live.

### Physical model

Make it **real on a specific platform**: exact SQL data types, indexes, partitioning, the actual `CREATE TABLE` DDL, and platform tuning. Names and structures match what the database engine will run.

### Why the levels

Working top-down keeps design honest: agree the *concepts* with the business, work out the *logical* structure without arguing about the database, then commit to a *physical* implementation. In this course, modules 02–08 live mostly at the **logical** level (keys, facts, dimensions, schemas), and module 10 (cloud & MPP) is where physical choices — distribution, partitioning, storage — pay off.
