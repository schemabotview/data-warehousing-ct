## Data marts — departmental subsets

A **data mart** is a subject-specific subset of the warehouse, scoped to a single department or business process — sales, finance, marketing. Where the enterprise warehouse holds the whole organization's integrated data, a mart gives one team a smaller, focused, faster slice, tailored to how they work.

### Why marts

- **Focus** — a team sees only the tables relevant to them, in their own vocabulary.
- **Performance** — smaller data and pre-aggregated tables mean faster queries.
- **Security** — access can be scoped to a department's mart.
- **Autonomy** — a team can iterate on its mart without affecting others.

### Two ways to build them

- **Dependent mart** — carved *from* the central enterprise warehouse (Inmon's top-down: warehouse first, marts derived). Consistent, because everything conforms to the warehouse.
- **Independent mart** — built directly from sources for one department, without a central warehouse first (Kimball's bottom-up starts from marts). Faster to stand up, but risks silos unless dimensions are conformed.

### Conformed dimensions keep marts consistent

If the sales mart and finance mart both use the *same* Customer and Date dimensions (conformed), their numbers line up and can be combined — the "bus" that ties independent marts into one coherent warehouse. (More in the dimensions module.)
