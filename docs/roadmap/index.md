# Roadmap

JU SeqWorkbench is being developed in stages. The roadmap below is intentionally conservative: it separates the current alpha workflow from post-alpha priorities and avoids treating long-term ideas as committed release features.

## Current — Alpha 0.1.x

The first public alpha is focused on a practical local desktop workflow for prepared sequence data.

### Implemented / current alpha scope

- FASTA and Sanger AB1 import/review
- editable NT/AA sequence viewer
- project save/load
- undo/redo and sequence/column editing workflows
- user-installed external MSA integration, with MAFFT recommended and Clustal Omega optional
- point visualization for selected AA, NT, or codon positions
- region-based variability and mutation inspection
- similarity clustering
- AA marker classification
- ID/name grouping
- CSV/TSV, FASTA, and figure export
- Korean / English UI support

### Before the first public alpha package

The remaining release gate is deliberately focused on the renderer bottleneck and release validation rather than adding new analysis features:

1. complete the developer viewport path for visible-range rendering and the required read-only data handoff
2. integrate it behind a controlled production path while preserving the current QPlainTextEdit renderer as a fallback/reference
3. verify Color/Dot display behavior, selection/editing, clipboard, Undo/Redo, scrolling/resize, and other renderer-parity cases with regression and manual tests
4. rerun the frozen benchmark under comparable conditions and confirm that the main bottleneck has been reduced enough for practical Alpha use
5. run the final packaging/license/manual release checks and publish the public Alpha package on GitHub

### Known alpha limitations

The first alpha is not intended to look or behave like a finished commercial release.

- larger alignments may still have practical limits, but the first public Alpha is now planned only after the current major full-document renderer bottleneck is reduced and validated
- visualization is currently functional first; visual polish and richer layouts are a post-alpha priority
- tree/relationship inspection is not yet part of the active alpha workflow
- ORF/segment-aware analysis is not yet implemented
- group-to-group comparison is planned for a later stage
- advanced annotation and broader sequence-management features remain deferred

These limitations will be stated openly when the alpha is released so that early feedback can focus on real workflow value rather than implying that the software is already feature-complete.

---

## Interaction-network roadmap

These diagrams are conceptual communication maps showing how JU SeqWorkbench modules interact now and how the workflow may expand over time.

- **solid lines** = primary / strong workflow interactions
- **dotted lines** = weaker, supporting, or cross-module interactions
- Beta and Full Release diagrams are **not fixed promises**; they may change with implementation results and user feedback

### Alpha — current

![JU SeqWorkbench Alpha interaction network](../../assets/images/roadmap/alpha-interaction-network.png)

### Beta — planned direction

![JU SeqWorkbench planned Beta interaction network](../../assets/images/roadmap/beta-interaction-network.png)

### Full Release — possible integrated structure

![JU SeqWorkbench possible Full Release interaction network](../../assets/images/roadmap/release-interaction-network.png)

---

## Next — Post-alpha / Beta direction

The first post-alpha development cycle is expected to focus on four connected areas.

### 1. Renderer follow-up and responsiveness

The initial viewport migration has moved into the pre-alpha release gate. Post-alpha renderer work is therefore expected to focus on stabilization and follow-up tuning rather than starting the migration from scratch.

Planned follow-up includes:

- tuning large-alignment responsiveness beyond the first Alpha threshold
- preserving and testing fallback/reference behavior during early migration
- tightening resize, scrolling, selection/editing, and invalidation edge cases
- reducing avoidable synchronous updates
- continuing to separate authoritative data/edit state from display/render state

### 2. Visualization modernization

The current Point and Region analysis calculations are useful, but the presentation layer should become easier to read and more modern.

Planned exploration includes:

- clearer Point plots
- richer Region plots
- improved mutation maps
- better visual hierarchy and summary views
- tighter movement between a summarized pattern and its underlying sequences

AI-assisted plotting may be used as a design/reference aid where useful, but a standalone AI analysis system is **not** part of the current committed roadmap.

### 3. Sequence relationship and tree inspection

For divergent viral datasets, site-by-site interpretation can be misleading if comparable sequence sets have not been identified first.

Planned exploration includes:

- NT/AA sequence-relationship inspection
- pairwise similarity/distance views
- simple tree-based inspection such as NJ/UPGMA-style distance trees
- selecting a clade/subset and returning to the alignment workflow
- using tree/relationship views both before and after point/region analysis

A simple distance tree should be described as a relationship/distance view, not automatically as full phylogenetic inference.

### 4. ORF- and segment-aware comparison

ORF and segment are different biological units and should remain distinct in the data model and UI.

Planned exploration includes:

- ORF mapping/selection
- segment-level comparison
- candidate homologous-region exploration
- choosing comparable ORFs, segments, clades, or regions before detailed site analysis

A likely future workflow is:

```text
Sequence set
→ relationship / similarity inspection
→ choose a comparable clade, ORF, segment, or region set
→ MSA / alignment review
→ point or region analysis
→ visualization / export
```

---

## Later — Toward a full release

The full-release direction is expected to integrate the workflows that prove useful during alpha and beta testing rather than add unrelated platform-scale features.

Current likely directions are:

- stable and optimized rendering
- modernized Point and Region visualization
- relationship/tree-assisted inspection
- ORF- and segment-aware analysis
- group-to-group comparison
- improved comparison summaries and export/report workflows
- continued project/session stability and editing reliability
- selected annotation/metadata features where they directly support sequence comparison

Cloud collaboration, enterprise administration, broad API ecosystems, and fully integrated AI analysis are **not** currently committed full-release requirements.

---

## Group-to-group comparison

Group comparison remains an important planned direction, but it is intentionally placed after the first post-alpha priorities above.

A future comparison workflow may include:

- Group A vs Group B
- marker-frequency differences
- region-level differences
- summary tables and comparison-oriented visualizations

The exact implementation will be shaped by alpha feedback and by the grouping/metadata workflow that proves most useful in practice.

---

## Long-term ideas

These remain ideas rather than committed milestones:

- richer annotation layers
- region/gene map visualization
- protein structure viewer/analysis linkage
- more advanced tree/phylogenetic workflows
- additional external-tool integrations
- more automated reporting

The priority remains a focused sequence-inspection and visualization workflow rather than becoming a general-purpose NGS platform.