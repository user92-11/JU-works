# JU SeqWorkbench Alpha

> **Independent personal project:** JU SeqWorkbench is developed independently as a personal software project. It is not an official product, service, or software project of my employer, and the views and development decisions presented here are my own.

JU SeqWorkbench Alpha is a desktop tool for practical Sanger/FASTA sequence inspection, focused on reducing tool-hopping between sequence review, editing, external alignment, grouping, point/region analysis, and export.

The project is intended for prepared sequence data and small-to-medium review workflows rather than large-scale NGS analysis, primary variant calling, read mapping, or genome assembly.

> Current status: `0.1.0-alpha` release preparation.
> The first public alpha package is not yet available from this repository page.

## Current focus

- Sanger AB1 and FASTA review
- editable NT/AA sequence inspection
- external MSA workflow with user-installed aligners
- Point Visualization for selected AA, NT, or codon sites
- Region Visualization for continuous intervals
- ID/name grouping, AA-marker classification, and similarity clustering
- project save/load and table/FASTA/figure export
- English/Korean UI and user guides

## Post-alpha priorities

The highest-priority updates after the first alpha release are planned around:

1. **Sequence relationship, ORF, and segment-aware analysis** — helping users inspect similarity/tree relationships and choose biologically comparable sequence groups or candidate homologous regions before site-by-site analysis when datasets are highly divergent.
2. **Visualization modernization** — improving mutation maps, region views, group-aware comparisons, visual hierarchy, and publication/report-oriented outputs in response to alpha feedback and current expectations for polished visual analysis.
3. **Rendering and performance optimization** — reducing unnecessary redraws and improving responsiveness for larger alignments, tables, and richer analysis views.

ORF and segment are treated as distinct biological levels. Planned relationship/homology-oriented features are exploratory post-alpha work and are not implemented in the current alpha source.

## Project focus

This tool is designed for workflows where sequence data has already been generated or prepared, such as:

- Sanger sequencing results
- FASTA sequence datasets
- aligned DNA/RNA/protein sequences
- gene or protein sequence comparison
- mutation and region-level variability inspection

For a fuller overview, development logs, planned directions, and feedback links, see the **personal development page** for this repository:

https://user92-11.github.io/JU-works/
