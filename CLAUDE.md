# CLAUDE.md — data-warehousing-ct

A **content repo**, not an app. It holds the **Data Warehousing** concept, authored **video-first** for the `graphl-render-app` app (sibling repo; the local mirror of the reference `graphl-movie`), which loads it **at runtime**. No render engine and no scenes live here — the app fetches this repo's `manifest.json` + notebooks + slides over the network and renders/records them. Read alongside the workspace map in `../CLAUDE.md`, the app contract in `../graphl-render-app/`, and the two references: the source notes `../ITC-bigdata/data-modeling-markdown/` (the ITC big-data data-modeling curriculum, prose only) and the sibling `-ct` template `../apache-spark-ct/`.

This is a video-target (`-ct`) content repo. It is authored **fresh** from the source notes — the 19 numbered `.md` files under `../ITC-bigdata/data-modeling-markdown/` (+ the two `.dbml` models) are **reference material**, not something to copy wholesale. The spine is **10 modules × ~10 sections** (see `README.md`); the source is ~80% dimensional-modeling mechanics framed by a data-warehouse context, reshaped here into the recognizable **Data Warehousing** course. Two modules are gap-filled beyond the notes: **09 · Loading the Warehouse (ETL/ELT)** and **10 · Cloud Data Warehouses & MPP** (which also absorbs the physical/query tuning from `16 - SQL Optimization`).

Concretely, each section's notebook is **authored fresh from the mapped `.md` source** (see the README's source map) — trimmed to the agreed spine, one `## ` section per notebook, diagram images dropped. **Narration is authored fresh** — every section gets a newly written `.tts`. **The owner generates the `.wav`s via Colab** (`scripts/colab_generate_audio.ipynb`, to be added); authoring never copies or commits audio.

There is **nothing to build, run, or test** here. The one executable (later) is the Colab tool that turns `tts/` scripts into `audio/` `.wav`s.

## Working agreement

Same as the app repo: **step by step, one small slice, review gate between each.** We settle the shape first (module spine → per-module sections → …), then fill content deliberately. No batch generation.

## The core contract (from graphl-movie — do not break)

1. **The notebook is the single source of truth** for a section's prose and code. `manifest.json` only *wires* — it must never duplicate notebook content.
2. **One notebook per section** (not per module): each `.ipynb` holds exactly one `## ` section. The section is the atomic unit across all four artifacts — `.ipynb` + `.tts` + `.slide` + `.wav` share the same `NN-SS-slug` stem — so a section can be authored and reviewed on its own. The manifest references each artifact by path; the notebook's single `## ` heading is the section title (mirror it in the manifest `heading`).
3. Each section is the unit the video steps through and has its own **`.ipynb`** (prose/code), **`.tts`** (narration script), **`.wav`** (generated audio), and — new for video — a **`.slide`** file (the authored right-pane: title + concise bullets). Module title/lede lives in `README.md` + the manifest, not in the section notebooks.
4. A section's diagram images (`![]()`) are **stripped** by the app — the React Flow **scene** replaces them.
5. **Scenes live in the app** (`graphl-render-app/src/scenes/*`). Here you only reference a scene **by id**, plus `highlight` (node ids that get the spotlight) and `focus` (a node id the camera frames) per section.

## Folder layout

```
data-warehousing-ct/
  notebooks/   # one .ipynb per SECTION (one ## section each) — source of truth for prose/code
  tts/         # one .tts per section (plain spoken narration script)
  audio/       # one .wav per section (generated from tts/ via Colab)
  slides/      # one .slide per section (authored right-pane title + bullets)
  scripts/     # Colab audio generator (added later)
  manifest.json  # wires sections → notebook / slide / scene / highlight / focus / audio
  CLAUDE.md · README.md
```

Naming: every artifact for a section shares the stem `<NN>-<SS>-<slug>`, where `NN` = module number, `SS` = section position (so a sorted glob stays in reading order): `notebooks/<NN>-<SS>-<slug>.ipynb`, `tts/…​.tts` → `audio/…​.wav`, `slides/…​.slide`.

The `.slide` format is a one-screen, scannable Markdown subset — a `# Title`, then `## ` sub-labels, short paragraphs, fenced ` ```code``` ` blocks, and numbered / `- ` lists, each key term marked with inline **`**bold**`** (rendered bright white, the rest a softer gray). **Keep the whole slide inside the fixed 1920×1080 frame:** the app's right pane does not scroll or auto-shrink type, so an over-long slide clips top and bottom — trim it to fit (drop connective prose the narration already carries) rather than expecting the engine to resize. Title may be punchier than the notebook `## ` heading.

## Scenes (app-side — to be authored)

Three scenes are referenced by the manifest and get authored app-side in `graphl-render-app/src/scenes/`, sourced from the `.dbml` models where possible:

- **`dw-architecture`** — the warehouse system map (sources → ETL → staging → storage → marts → BI). Used by modules 01, 09, 10.
- **`star-schema`** — the shared `FACT_SALES` + dimensions master map, sourced from `../ITC-bigdata/data-modeling-markdown/jabra-spain-dw-model.dbml`. Used by modules 02–06, 08.
- **`datavault-model`** — hubs / links / satellites, sourced from `../ITC-bigdata/data-modeling-markdown/jabra-spain-datavault-model.dbml`. Used by module 07.

None exist yet. Per-section `focus`/`highlight` get added to the manifest as each scene lands and its node ids are known. **Keep node ids stable** so `highlight`/`focus` wiring transfers.

## Curriculum

The course outline (module spine + per-module sections + the `.md` → module source map) lives in [`README.md`](./README.md) — the single human-facing source for structure while we plan; `manifest.json` is the machine source of truth. Don't duplicate the module/section list here.

**Agreed target:** 10 modules × ~10 sections, each authored as one standalone narrated video (one dense scene + a linear section walkthrough). The `17 - Dimensional Modeling — Interview Q&A` sheet is parked (Q&A is a weak fit for the narrated-scene format) but kept as a prose reference.

## Status

Scaffolded + spine settled (`README.md`: 10 modules × 100 sections). `manifest.json` wires the full spine (heading + notebook/slide/scene paths, `spine:true`, `role:hook` on each §01); `audio` and per-section `focus`/`highlight` are added as sections and scenes are authored. Folder scaffold in place (`notebooks/ tts/ audio/ slides/ scripts/`, empty). Concept wired into the app catalog (`graphl-render-app/src/content/catalog.ts` → `data-warehousing`, replacing the old `data-modeling` placeholder). **Nothing authored yet**; scenes not yet built. Next: author module 01 end-to-end + port the `dw-architecture` scene.
