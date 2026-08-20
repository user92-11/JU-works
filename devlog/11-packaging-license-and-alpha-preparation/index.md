# Devlog 11 — Packaging, License, and Alpha Preparation

This devlog covers the final preparation work for the first JU SeqWorkbench Alpha package and the development priorities expected immediately after release.

The current application version is `0.1.0-alpha`.

This page is still a development note rather than a sales or release announcement. The first public alpha package has not yet been posted.

---

## Current alpha-preparation status

Recent release-preparation work has focused less on adding new analysis features and more on making the existing workflow safer and easier to test as a packaged desktop application.

Current preparation includes:

- JU SeqWorkbench Alpha branding and shared application metadata
- a centered branded startup splash shown before the main application finishes loading
- expanded English/Korean localization across the main UI, Help pages, and several analysis dialogs
- Korean and English user guides
- safer clipboard and sequence-editing behavior
- project save/load and export-path stabilization
- Sanger AB1 import and trace review
- external MSA execution using separately installed tools
- Point Visualization and Region Visualization workflows
- Similarity Clustering, AA Marker Classification, and ID/name Grouping
- large-analysis preflight checks and progress/busy feedback
- modeless analysis/configuration windows where appropriate
- automated smoke testing for release-sensitive workflows

The current source-level alpha smoke suite passes 29 `scripts/smoke_*.py` checks in the latest release-preparation run.

---

## Packaging direction

JU SeqWorkbench is being prepared as a Windows desktop application using PySide6 and PyInstaller.

The packaging goal is to keep the application core and external analysis executables clearly separated.

For the current alpha workflow:

- MAFFT is the recommended external aligner
- Clustal Omega is optional
- external aligner binaries are not bundled with JU SeqWorkbench
- the application does not automatically download those external executables
- users connect a separately installed local executable through the application workflow

This separation also makes it easier to keep external-tool troubleshooting and license obligations distinct from the application core.

---

## Open-source and license review

Open-source license review remains part of release preparation.

The current design intentionally keeps external alignment programs outside the application package and communicates with them through `subprocess` and temporary FASTA input/output files.

The packaged application itself also needs appropriate third-party license/notice material for libraries that are redistributed with it.

The goal of the alpha is not to claim that license review is permanently finished. The goal is to document the current dependency boundary clearly and prepare a package that can be reviewed and updated before broader or commercial distribution.

---

## Local-data direction

JU SeqWorkbench is designed as a local desktop workflow.

Sequence files, working projects, analysis tables, and local external-tool execution are intended to remain on the user's machine unless the user separately chooses to export or share data elsewhere.

This is particularly relevant for research workflows where unpublished or institution-restricted sequence data should not be uploaded to an external AI or web service simply to perform routine inspection.

---

## First alpha scope

The first alpha is intentionally narrower than the long-term plan.

Its purpose is to validate whether the existing connected workflow is useful:

```text
FASTA / AB1
→ sequence review and editing
→ external MSA / alignment review
→ grouping / typing / similarity inspection
→ Point or Region Visualization
→ table / figure / FASTA export
```

The first alpha should therefore be evaluated mainly on:

- whether the workflow is understandable
- whether the current editing and analysis paths remain consistent
- which functions users repeatedly return to
- where the UI is confusing or slow
- which visualization and comparison formats are missing

---

## Feedback will be more specific than “please report bugs”

Alpha feedback will explicitly ask for visualization and analysis ideas, not only errors.

Useful requests include:

- “I want to compare these two groups in this format.”
- “I want to see this ORF or segment relationship before looking at individual sites.”
- “This mutation map from another tool/paper is easier to interpret.”
- “I need this table/figure repeatedly for my sequence-review workflow.”

Screenshots or references from papers and other tools can be useful design references, provided no confidential sequence data is shared publicly.

---

## Highest-priority post-alpha updates

The first update cycle after the public alpha is expected to focus on three areas before expanding into a broader list of features.

### 1. Sequence relationship, ORF, and segment-aware analysis

The current Point Visualization workflow is strongest when users already know that aligned positions are biologically comparable.

More complex or highly divergent viral datasets may require an earlier relationship layer. The same named ORF or corresponding segment does not automatically guarantee that direct site-by-site comparison is the best first analysis.

The post-alpha plan therefore prioritizes exploration of:

- NT- and AA-level sequence relationship views
- tree-based inspection
- ORF-aware and segment-aware analysis while keeping those biological levels distinct
- selection of comparable sequence groups before downstream analysis
- candidate homologous-region exploration
- smoother handoff from relationship inspection to MSA, Point Visualization, and Region Visualization

The intended direction is closer to:

```text
Sequence set
→ relationship / similarity inspection
→ select comparable clade / ORF / segment / region
→ alignment review
→ point or region analysis
→ visualization / export
```

These features are planned work and are not presented as implemented in `0.1.0-alpha`.

### 2. Visualization modernization

The existing visualization layer is useful for alpha workflow validation, but visual presentation itself will be treated as a major development target.

Current AI-assisted plotting and analysis tools can generate polished figures quickly, which has raised user expectations for clarity and visual quality. The goal for JU SeqWorkbench is not to compete by generating decorative charts. It is to make scientific sequence views easier to read, compare, and connect back to the underlying data.

Planned investigation includes:

- cleaner default layouts and visual hierarchy
- improved mutation maps and region summaries
- relationship-aware and group-aware comparison views
- more useful publication/report-oriented figure options
- tighter navigation between a visual pattern and the sequences that produced it
- collecting concrete visualization requests from alpha users rather than guessing every desired format in advance

### 3. Rendering and responsiveness optimization

Richer analysis and visualization increase rendering cost.

Post-alpha work will therefore also review performance in the viewer, result tables, mutation maps, and other large visual outputs.

Planned areas include:

- reducing unnecessary full redraws
- improving responsiveness for larger alignments and result tables
- reviewing visible-range or incremental rendering where appropriate
- keeping expensive analysis and UI rendering boundaries clearer

The goal is to improve visual quality without making the application feel heavier or less responsive.

---

## What comes after those priorities?

Group-to-group comparison remains an important planned analysis feature and should connect naturally with the relationship layer.

For example, a future workflow may allow a user to identify two clades or user-defined groups and then compare site/region mutation patterns between them.

Other planned directions such as deeper annotation support, more external tools, and additional reporting features remain relevant, but they are not the first priority immediately after the initial alpha release.

---

## Release-preparation principle

The first alpha does not need to contain every planned feature.

The immediate goal is to publish a coherent build, collect structured feedback, and then update the program quickly around the most important missing analysis layer and visualization needs.

That is why sequence relationship / ORF / segment-aware analysis, visualization modernization, and rendering performance are being documented now as the first major post-alpha priorities rather than being rushed into the initial package.
