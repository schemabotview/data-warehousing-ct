## Keys — super, candidate, primary, alternate

Splitting tables only works if each row can be **uniquely identified** and tables can **link** to one another. That's the job of keys. Four related terms describe how uniqueness narrows down to the one column we build on.

We'll use a **STUDENT** table: `(student_id, email, roll_no, dept_id, name)`. Several columns can uniquely identify a student — and each plays a different role.

### Super key — any unique set

A **super key** is *any* set of columns that uniquely identifies a row. `student_id` alone is a super key. So is `(student_id, name)` — still unique, just with a redundant extra column. Super keys are permissive: adding more columns to a unique set keeps it unique.

### Candidate key — a minimal super key

A **candidate key** is a super key with **no redundant columns** — remove any column and it stops being unique. `(student_id, name)` is *not* a candidate key, because dropping `name` still leaves a unique key. In STUDENT, `student_id`, `email`, and `roll_no` are each candidate keys: three genuine, minimal ways to identify a student. A table can have several candidate keys, and a candidate key can itself be a combination of columns.

### Primary key — the one you choose

The **primary key** is the candidate key you **elect** to identify rows throughout the design. It must be **unique** and **NOT NULL**, and it's the key other tables reference. Here we choose `student_id` — stable, compact, always present.

### Alternate keys — the candidates not chosen

The candidate keys you didn't pick become **alternate keys**. They still uniquely identify a row (you'd typically enforce a UNIQUE constraint on them), they're just not *the* identifier the model is built on. With `student_id` as primary, `email` and `roll_no` are alternate keys.

### How they nest

- **Super key** — unique, possibly with extra columns.
- **Candidate key** — minimal super key (every super key contains one).
- **Primary key** — the chosen candidate key (exactly one per table).
- **Alternate key** — the remaining candidate keys.

Two special cases deserve their own sections: a primary key built from **several columns at once** (a composite key), and a key that **links to another table's** primary key (a foreign key) — next.
