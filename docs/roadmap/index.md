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

The remaining release gate is deliberately small:

1. finish the final open-source license/distribution review
2. apply any required packaging or notice corrections
3. run a final manual alpha test using representative workflows
4. publish the public alpha package on GitHub

### Known alpha limitations

The first alpha is not intended to look or behave like a finished commercial release.

- larger alignments and some analysis views can still render slowly
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

### 1. Rendering and responsiveness

Performance work comes first because richer analysis is only useful if the viewer remains responsive.

Planned work includes:

- reducing unnecessary redraws
- improving large-alignment responsiveness
- visible-range or incremental rendering where appropriate
- reducing avoidable synchronous updates
- continuing to separate calculation state from display/render state

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