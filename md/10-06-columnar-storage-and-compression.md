## Columnar storage & compression

If one thing explains why analytic queries fly on these platforms, it's **columnar storage**. It's the physical layout that matches the analytic workload — and it quietly retires the B-tree index.

### Row vs column layout

- **Row storage** (OLTP) keeps each row's columns together — ideal for "fetch one whole row," the transactional pattern.
- **Columnar storage** keeps each **column** together — ideal for analytics, which read **a few columns across many rows**.

```
row:      [1,Ana,Madrid,600] [2,Leo,Sevilla,300] …
columnar: [1,2,…] [Ana,Leo,…] [Madrid,Sevilla,…] [600,300,…]
```

### Why it's fast for analytics

- **Read only the columns you need.** `SUM(line_total)` reads just the `line_total` column and skips the twenty others — a fraction of the I/O. (This is why `SELECT *` is a warehouse sin: it drags every column off disk.)
- **Compression.** A column holds values of one type with low variety, so it compresses **extremely** well — run-length, dictionary, delta encoding. Less storage, and less data to read = **less I/O**, the real bottleneck.
- **Vectorised execution.** The engine processes compressed column blocks in bulk, many values per CPU instruction.

### It replaces the index

Row stores lean on **B-tree indexes** to avoid full scans. Columnar warehouses don't: reading one column of a billion rows, compressed and split across MPP nodes, is already cheap — and **data skipping** (zone maps, next section) handles the "find the matching rows" job an index used to do. So the module-01 idea "add an index" becomes, here, "**store columnar and cluster well**."

> Columnar storage keeps each column together, so a query reads only the columns it needs, compresses them hard, and scans them vectorised across MPP nodes. Massive I/O savings — and the reason indexes give way to columnar + data-skipping.
