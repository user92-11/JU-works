# JU SeqWorkbench Alpha

Sanger/FASTA sequence inspection, editing, grouping, visualization, and export in one local desktop workflow.

## Independent personal development project

**JU SeqWorkbench is developed independently as a personal software project. It is not an official product, service, software project, or development activity of my employer.** The views, design choices, documentation, and development decisions presented on this site are my own.

JU SeqWorkbench Alpha is being developed for practical sequence-review work where sequence data has already been generated or prepared. The goal is to reduce repeated tool-switching between sequence inspection, editing, alignment review, grouping, mutation/variability analysis, visualization, and export.

It is **not** intended to replace large-scale NGS analysis platforms, read mapping, primary variant calling, or genome assembly pipelines.

> **September 2026 update:** `0.1.0-alpha` is in final stabilization. Before the first public release, the current sequence renderer is being optimized with a viewport-based approach, followed by function-parity/regression checks and final manual release validation.

## Why this project?

Routine Sanger/FASTA review can still involve moving between several tools for small but repeated tasks:

- checking aligned sequences
- editing IDs or sequence rows
- reviewing selected mutation sites
- comparing continuous regions
- grouping sequences by ID, marker rules, or similarity
- running an external MSA engine
- exporting tables, FASTA files, and figures

JU SeqWorkbench aims to connect those steps into one focused desktop workflow.

### Workflow preview

![Workflow preview](assets/images/devlog/devlog00-overview-core.gif)

---

## Current Alpha scope

The current `0.1.x` workflow includes:

- FASTA and Sanger AB1 import/review
- chromatogram trace inspection
- editable NT/AA sequence viewing
- project save/load
- undo/redo and row/column editing workflows
- user-installed external MSA integration
- MAFFT as the recommended alpha aligner path
- optional local Clustal Omega integration
- Point Visualization for selected AA, NT, or codon sites
- Region Visualization for continuous intervals or multiple regions
- similarity clustering
- AA marker classification
- ID/name grouping
- CSV/TSV, FASTA, grouped FASTA, and figure export
- Korean / English UI support
- alpha-level preflight and busy-state guardrails for larger analysis jobs

The analysis path is designed to use the current working sequence state rather than silently falling back to stale imported data.

---

## Known Alpha limitations

The first alpha is intentionally being released before every presentation and performance issue is polished.

The main limitations that will be stated openly with the release are:

- larger alignments and some analysis views may still render slowly; renderer optimization is currently in progress
- current visualization is functional-first and will be visually modernized after alpha feedback
- relationship/tree inspection is not yet active in the alpha workflow
- ORF/segment-aware analysis is not yet implemented
- group-to-group comparison is planned for a later stage
- annotation and broader sequence-management features remain deferred

The goal of the first public alpha is workflow validation: **does the current sequence-review process save time, and which analysis/visualization views are actually worth improving next?**

---

## Performance baseline

A **JU-only pre-renderer performance baseline** has now been frozen before the planned renderer migration. It is intended as a development reference for later before/after optimization work, not as a ranking against other software.

The public baseline contains 5 synthetic alignment fixtures × 8 renderer metrics and reports median/min/max timings under the documented Windows/Qt test conditions.

[View the Performance Baseline](docs/performance/)

---

## Alpha → Beta → Full Release direction

The project roadmap is intentionally conservative. Post-alpha development is focused on improving the existing workflow before adding unrelated platform-scale features.

### Interaction-network roadmap

The diagrams below are a visual interpretation of how the current modules interact and how the workflow may expand over time. Solid lines represent primary/strong workflow interactions; dotted lines represent weaker, supporting, or cross-module interactions.

**Alpha — current interaction network**

![JU SeqWorkbench Alpha interaction network](assets/images/roadmap/alpha-interaction-network.png)

**Planned Beta interaction network**

![JU SeqWorkbench planned Beta interaction network](assets/images/roadmap/beta-interaction-network.png)

**Possible Full Release interaction network**

![JU SeqWorkbench possible Full Release interaction network](assets/images/roadmap/release-interaction-network.png)

> Beta and Full Release diagrams are conceptual roadmaps. Their exact structure may change based on implementation results, alpha feedback, and workflow priorities.

### Alpha — current foundation

```text
FASTA / AB1 input
→ sequence review and editing
→ external MSA / alignment review
→ grouping / typing / similarity
→ point or region analysis
→ visualization / export
```

### Beta — first post-alpha priorities

The current expected priorities are:

1. **Rendering and performance optimization**
   - reduce unnecessary redraws
   - improve responsiveness for larger alignments/results
   - investigate visible-range or incremental rendering

2. **Visualization modernization**
   - improve Point plots
   - improve Region plots
   - modernize mutation maps and summary layouts
   - make it easier to move from a visual pattern back to the underlying sequences

3. **Sequence relationship and tree inspection**
   - NT/AA sequence relationship views
   - pairwise similarity/distance inspection
   - simple NJ/UPGMA-style distance-tree views
   - clade/subset selection linked back to the alignment workflow

4. **ORF- and segment-aware analysis**
   - ORF mapping/selection
   - segment-level comparison
   - candidate homologous-region exploration
   - selection of comparable clades/ORFs/segments before detailed site analysis

Group-to-group comparison remains important, but is currently positioned **after** these first post-alpha priorities.

### Toward a full release

The likely full-release direction is to integrate and stabilize the workflows that prove useful during alpha/beta testing:

- optimized rendering and responsiveness
- modernized Point/Region visualization
- relationship/tree-assisted inspection
- ORF- and segment-aware analysis
- group-to-group comparison
- stronger comparison summaries and export/report workflows
- selected annotation/metadata features where they directly support sequence comparison

Cloud collaboration, enterprise administration, large API ecosystems, and fully integrated AI analysis are **not** currently committed full-release requirements.

[See the detailed roadmap](docs/roadmap/)

---

## Sanger AB1 workflow

The AB1 workflow supports opening Sanger trace files, displaying chromatogram traces, reviewing the basecalled sequence, choosing original or reverse-complement orientation, trimming an import range, and bringing the selected sequence into the main viewer workflow.

---

## External MSA

JU SeqWorkbench does not reimplement a multiple-sequence aligner. The alpha workflow connects to separately installed external aligners through temporary FASTA files and returns the aligned result to the viewer.

- **MAFFT** — recommended alpha path
- **Clustal Omega** — optional when the user already has a local executable

External aligner binaries are not bundled or automatically downloaded by the current alpha build.

---

## Visualization and inspection

The visualization workflow deliberately separates two different questions:

- **Point Visualization** — selected AA, NT, or codon positions
- **Region Visualization** — continuous intervals or multiple regions

The current plots are intended to validate the analysis workflow first. Visual hierarchy, comparison layouts, mutation maps, and publication/report-oriented presentation will be improved after real user feedback.

---

## Feedback wanted

Feedback from people who review Sanger results, FASTA files, small MSA datasets, viral sequences, or amplicon sequence sets is especially useful.

Useful feedback includes:

- which sequence-review steps feel repetitive or inconvenient
- whether FASTA → MSA → inspection → export matches a real workflow
- which Point or Region visualization formats you would actually use
- how you would want to compare groups, ORFs, segments, or related sequence sets
- whether relationship/tree inspection should appear before detailed site/region analysis
- examples of figures or tables from papers or other tools that communicate the result better

If possible, describe **what data should be compared and what you want to see from it**.

Please do not upload confidential or unpublished sequence data publicly.

---

## Development logs

### Foundation

- [Devlog 01 — Project Direction](./devlog/01-project-direction/)
- [Devlog 02 — Data Layer and Working State](./devlog/02-data-layer-and-working-state/)
- [Devlog 03 — Editing and Interaction Model](./devlog/03-editing-and-interaction-model/)

### Analysis and visualization

- [Devlog 04 — Classification and Grouping](./devlog/04-Classification%20and%20grouping/)
- [Devlog 05 — Visualization and Inspection](./devlog/05-visualization-and-inspection/)
- [Devlog 06 — Sequence Relationship and Tree-Based Inspection](./devlog/06-tree-based-inspection-and-phylogenetic-workflow/)
- [Devlog 07 — Annotation and Region Metadata](./devlog/07-annotation-and-region-metadata-layer/)

### Output, Sanger, and alpha preparation

- [Devlog 08 — Export and Reporting Workflow](./devlog/08-export-and-reporting-workflow/)
- [Devlog 09 — Sanger AB1 Workflow](./devlog/09-sanger-AB1-workflow/)
- [Devlog 10 — External MSA Workflow](./devlog/10-external-MSA-and-tool-manager/)
- [Devlog 11 — Packaging, License, and Alpha Preparation](./devlog/11-packaging-license-and-alpha-preparation/)

### Planned / upcoming development topics

The following areas are planned or under active exploration. Dedicated devlog pages will be linked when implementation and validation are mature enough to document clearly.

- **Renderer and viewport optimization** — current pre-alpha work to improve responsiveness while preserving editing, coloring, Dot mode, highlighting, and selection behavior
- **Visualization modernization** — clearer Point/Region views, mutation maps, comparison summaries, and more useful links back to the underlying sequences
- **ORF- and segment-aware analysis** — treating ORFs and genome segments as distinct biological levels for comparison and selection before site-level analysis
- **Group-to-group comparison** — comparing selected sequence groups and their variability/pattern summaries
- **ID normalization and reusable subset filtering** — batch-cleaning inconsistent FASTA IDs and reusing standardized IDs to extract different sequence subsets efficiently
- **Structure-linked sequence inspection** — future exploration of loading PDB/mmCIF structures and mapping selected variants or group differences onto a structure viewer

These are roadmap topics rather than promises for a specific release, and their order may change based on alpha feedback and implementation results.

---

## Documentation

### User guides

- [Korean User Guide — online](docs/user-guide/user_guide_ko.md)
- [English User Guide — online](docs/user-guide/user_guide_en.md)
- <a href="docs/user-guide/JU_SeqWorkbench_User_Guide_KO.pdf" download>Download Korean User Guide (PDF)</a>
- <a href="docs/user-guide/JU_SeqWorkbench_User_Guide_EN.pdf" download>Download English User Guide (PDF)</a>

### Project documentation

- [Performance Baseline](docs/performance/)
- [Roadmap](docs/roadmap/)
- [Scope](docs/scope/)
- [Alpha Limitations & Cautions](docs/limitations/)
- [Workflow Concept](docs/workflow/)
- [Planned Features](docs/features/)

---

## Feedback links

- Bug reports / reproducible errors / feature requests: [GitHub Issues](https://github.com/user92-11/JU-works/issues)
- Questions / ideas / visualization requests / general feedback: [GitHub Discussions](https://github.com/user92-11/JU-works/discussions)

Please do not upload confidential or unpublished sequence data publicly.
