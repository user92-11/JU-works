# JU SeqWorkbench Alpha

Sanger/FASTA sequence inspection in one desktop workflow.

JU SeqWorkbench Alpha is a desktop tool for reducing tool-hopping between sequence review, editing, grouping, site-based mutation checks, region-based visualization, external MSA, and export.

The project is aimed at practical sequence-review workflows where data has already been generated or prepared. It is not intended to replace large-scale NGS analysis platforms, read mapping, variant calling, or genome assembly pipelines.

> **August 2026 update:** `0.1.0-alpha` is in release preparation. English/Korean localization has expanded across the main UI and analysis workflow, a branded startup splash and bilingual user guides are in place, and the current source-level alpha smoke suite passes 29 automated checks. The first public alpha package is still being prepared and is not yet available from this page.

## Why this project?

Many existing sequence-analysis tools are powerful, but routine Sanger/FASTA review can still require moving between separate programs for small but repeated tasks:

- checking aligned sequences
- editing IDs or sequence rows
- reviewing selected mutation sites
- comparing continuous regions
- grouping sequences by ID, marker rules, or similarity
- running an external MSA engine
- exporting tables, FASTA files, and figures

JU SeqWorkbench aims to connect those steps into one practical desktop workflow.

### Workflow preview

The current alpha workflow keeps sequence inspection, editing, analysis, and export close together.

![Workflow preview](assets/images/devlog/devlog00-overview-core.gif)

### Current focus

- FASTA and Sanger AB1 review
- editable NT/AA sequence inspection
- external MSA workflow with user-installed aligners
- point visualization for selected AA, NT, or codon sites
- region-based mutation and variability inspection
- ID/name grouping, AA-marker classification, and similarity clustering
- project save/load and CSV/TSV/FASTA/figure export
- alpha-level guardrails for larger analysis jobs

### Not the focus

This project is not intended for large-scale NGS analysis, read mapping, primary variant calling, or genome assembly.

## Current status

**JU SeqWorkbench `0.1.0-alpha` — release preparation**

The current source is being checked for packaging, documentation, localization, external-tool boundaries, and reproducible alpha workflows. A public alpha package has not yet been posted.

---

## Post-alpha priorities

The first alpha is intentionally focused on the workflow that already works: prepared sequence data → inspection/editing → alignment → grouping/typing/similarity → point or region analysis → export.

The highest-priority updates after the first alpha release are expected to expand the analysis layer in three directions.

### 1. Sequence relationship, ORF, and segment-aware analysis

Point-by-point comparison is useful when sequences already share a meaningful alignment and coordinate relationship. More divergent viral datasets can require an earlier relationship step before individual sites are interpreted.

Planned exploration therefore includes:

- sequence-relationship inspection before site-by-site comparison
- tree-based inspection as both an upstream grouping aid and a downstream visualization
- ORF-level and segment-level analysis workflows while keeping ORF and segment concepts distinct
- NT- and AA-level similarity views for divergent datasets
- candidate homologous-region exploration rather than assuming that similarly named regions are directly comparable

A likely future workflow is:

```text
Sequence set
→ relationship / similarity inspection
→ choose a comparable clade, ORF, segment, or region set
→ MSA / alignment review
→ point or region analysis
→ visualization / export
```

These features are planned after the first alpha and are not presented as implemented functionality yet.

### 2. Visualization refresh

The current plots are useful for workflow validation, but visualization quality and readability will be treated as a major post-alpha development area.

The goal is not only to make plots look newer. Future work will examine clearer visual hierarchy, more modern mutation/region layouts, group-aware comparison views, and better ways to move between summarized patterns and the underlying sequences.

Current AI-assisted analysis and plotting tools have also raised user expectations for fast, polished visual outputs. JU SeqWorkbench will use alpha feedback to identify which plot styles, mutation maps, comparison layouts, and publication/report-oriented views are most useful in real sequence-review work.

### 3. Rendering and performance optimization

As analysis and visualization become richer, rendering cost becomes more important. Post-alpha work will therefore include reducing unnecessary redraws, improving responsiveness with larger alignments and result tables, and reviewing visible-range or incremental rendering strategies where they are useful.

Group-to-group comparison remains an important planned direction, but the first post-alpha development cycle will prioritize sequence relationship / ORF / segment-aware analysis, visualization modernization, and rendering performance.

---

## Feedback wanted

This project is entering an alpha workflow-validation stage. Feedback from people who review Sanger results, FASTA files, small MSA datasets, viral sequences, or amplicon sequence sets is especially useful.

**Visualization and analysis requests are particularly welcome.** Useful feedback includes:

- Which parts of sequence review feel repetitive or inconvenient
- Whether FASTA → MSA → inspection → export matches a real workflow
- Which mutation-map or region-visualization formats you would actually use
- How you would want to compare groups, ORFs, segments, or related sequence sets
- Whether a tree or sequence-relationship view should appear before site/region analysis
- Examples of figures or tables from papers or other tools that communicate the result better
- Features that would make the alpha useful enough to keep using repeatedly

If possible, describe **what data should be compared and what you want to see from it**. Example screenshots or references from papers/tools are welcome as design references.

Please do not upload confidential or unpublished sequence data publicly.

---

## Project overview

This project started from a practical problem: routine sequence inspection often requires moving between several tools for sequence viewing, manual checking, alignment review, mutation inspection, grouping, and figure/table preparation.

The goal is not to replace every existing bioinformatics tool. Instead, JU SeqWorkbench aims to make common inspection workflows faster, more connected, and easier to repeat.

The current implemented development focus includes:

- opening and reviewing FASTA sequence files
- opening and importing Sanger AB1 reads
- checking basecalled sequences and chromatogram traces
- editing and organizing sequence data
- reviewing external alignment results
- grouping sequences by ID labels, AA marker rules, or similarity
- inspecting selected AA, NT, or codon sites
- visualizing continuous sequence regions
- exporting tables, FASTA files, and figures

---

## Core workflow

```text
FASTA / AB1 input
→ sequence viewing and editing
→ external MSA / alignment review
→ grouping / typing / similarity inspection
→ point or region visualization
→ table / figure / FASTA export
```

This workflow is designed for practical sequence-checking situations where users need to move quickly from prepared sequence data to interpretable tables and figures.

---

## Current feature areas

### Sequence viewer and editing

The viewer supports sequence viewing, row and column selection, safe copy/paste workflows, column-based editing, undo/redo behavior, single-sequence editing, project save/load, and independent duplicate-view inspection.

The data path is designed so that analysis should use the current working sequence state rather than silently falling back to stale imported data.

### Sanger AB1 workflow

The AB1 workflow supports opening Sanger trace files, displaying chromatogram traces, reviewing the basecalled sequence, choosing original or reverse-complement orientation, trimming an import range, and bringing the selected sequence into the main viewer workflow.

### External MSA

JU SeqWorkbench does not reimplement a multiple-sequence aligner. The alpha workflow connects to separately installed external aligners through temporary FASTA files and returns the aligned result to the viewer.

MAFFT is the recommended alpha path. Clustal Omega is optional when the user already has a local executable. External aligner binaries are not bundled or automatically downloaded by the current alpha source.

### Grouping and typing

The project includes ID/name-based grouping, amino-acid marker classification, and similarity clustering. These functions help users organize a sequence set before deeper inspection or export.

### Visualization and inspection

The current visualization workflow has two major layers:

- **Point Visualization** for selected AA, NT, or codon positions
- **Region Visualization** for continuous intervals or multiple regions

This separation is intentional because selected-position inspection and continuous-region inspection answer different biological questions.

### Export and reporting

Current export workflows focus on practical outputs such as CSV/TSV tables, FASTA files, grouped FASTA outputs, and figure exports. Full automated report generation remains future work.

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

---

## Documentation

- [Korean User Guide](docs/user-guide/user_guide_ko.md)
- [English User Guide](docs/user-guide/user_guide_en.md)
- [Roadmap](docs/roadmap/)
- [Scope](docs/scope/)
- [Alpha Limitations & Cautions](docs/limitations/)
- [Workflow Concept](docs/workflow/)
- [Planned Features](docs/features/)

---

## Feedback

JU SeqWorkbench Alpha feedback is welcome.

- Bug reports / reproducible errors / feature requests: [GitHub Issues](https://github.com/user92-11/JU-works/issues)
- Questions / ideas / visualization requests / general feedback: [GitHub Discussions](https://github.com/user92-11/JU-works/discussions)

Please do not upload confidential or unpublished sequence data publicly.
