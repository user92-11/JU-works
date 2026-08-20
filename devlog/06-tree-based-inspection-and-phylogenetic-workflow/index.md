# Devlog 06 — Sequence Relationship and Tree-Based Inspection

This devlog originally treated tree-based inspection mainly as a downstream step after sequence editing, grouping, and site/region mutation review.

That direction is still useful, but alpha-stage feedback has highlighted a second and more important role for some viral datasets: **sequence relationship may need to be examined before site-by-site comparison begins.**

The planned direction is therefore broader than simply adding a tree figure.

---

## Why relationship inspection may need to come first

Point and region analysis work best when the sequences already share a meaningful alignment and coordinate relationship.

For relatively similar sequences, this assumption is often reasonable. A selected aligned column can be interpreted as the same biological position across the sequence set.

More divergent viral datasets can be different.

Sequences may carry the same ORF name or belong to corresponding genomic segments while still being divergent enough that a simple same-position comparison is not the best first step. Insertions, deletions, long-term divergence, different genome organizations, or segment-specific evolutionary histories can make the biological relationship more important than the nominal coordinate label.

In those cases, the workflow may need to ask:

1. Which sequences are actually close enough to compare directly?
2. Which clades or sequence groups are present?
3. Which ORFs or segments should be treated as corresponding analysis units?
4. Is there a candidate homologous region that should be aligned and inspected separately?
5. Only then, which point or region differences are meaningful?

---

## ORF and segment are different analysis levels

A key design point is that **ORF and segment will not be treated as interchangeable range labels.**

A segment is a physical genome component in a segmented virus. An ORF is a coding region that may lie within a segment or within a non-segmented genome. One segment can contain more than one ORF, and corresponding ORFs do not imply that segment numbering or organization must be identical across all viruses.

This matters for software design because future analysis should preserve the distinction between:

- isolate / genome
- segment
- ORF / protein
- aligned or candidate homologous region

Keeping those levels separate should make later tree, similarity, grouping, and visualization features easier to extend without forcing biologically different concepts into one generic range model.

---

## Planned post-alpha workflow

The first public alpha will keep the currently implemented workflow focused on already prepared sequence sets.

The highest-priority analysis update after that release is expected to explore a relationship-first workflow such as:

```text
Sequence set
→ NT / AA relationship or similarity inspection
→ tree / clustering view
→ choose a comparable clade, ORF, segment, or region set
→ MSA / alignment review
→ point or region analysis
→ visualization / export
```

The exact UI and algorithms are not fixed yet. Alpha feedback will be used to decide how much of this should be automated and how much should remain user-directed.

---

## Tree as an inspection tool, not only an output

A tree can be useful at two stages.

### Upstream relationship inspection

A tree or related similarity view can help users see whether a dataset contains strongly separated groups before they interpret individual positions.

Potential uses include:

- finding major sequence groups or clades
- deciding whether the full dataset should be analyzed together
- selecting a subset for a separate MSA or region analysis
- comparing NT-level and AA-level relationships when nucleotide divergence is high
- identifying segment-specific or ORF-specific relationship patterns

### Downstream interpretation

After mutation or region analysis, a tree can also help users ask whether an observed mutation pattern follows the same sequence relationships.

A longer-term goal is therefore to connect tree selection with the sequence viewer rather than treating a tree as a detached static image.

---

## Homology and similarity wording

The planned feature should be careful about terminology.

Sequence similarity is measurable. Homology describes an evolutionary relationship and should not be presented as a simple percentage score.

For that reason, early JU SeqWorkbench features are more likely to use terms such as:

- sequence relationship
- similarity search
- tree-based inspection
- candidate homologous region

rather than claiming that the program automatically proves homology from a single score.

Potential later exploration may include local similarity/search workflows or external-tool integration if alpha feedback shows that this is important.

---

## Relationship-aware visualization

This direction also changes the role of visualization.

The current alpha focuses mainly on selected positions and defined regions. A relationship-aware layer could later make it possible to:

- order mutation maps by tree or similarity structure
- select a clade and immediately inspect its point/region pattern
- compare the same ORF or segment across biologically selected groups
- show where a candidate comparable region exists before plotting site-level differences
- connect sequence, tree, grouping, and visualization views more directly

This is expected to be more useful than adding a phylogenetic tree as an isolated menu item.

---

## Priority after the first alpha

For the first post-alpha development cycle, the intended priority is:

1. **sequence relationship / ORF / segment-aware analysis**
2. **tree and candidate homologous-region inspection**
3. **visualization modernization and relationship-aware visual layouts**
4. **rendering and responsiveness optimization as the views become richer**
5. **group-to-group comparison as the next analytical extension**

This ordering may change after real alpha feedback, but it reflects the current development questions more accurately than the earlier plan of adding tree inspection only as a downstream feature.

> These are planned post-alpha directions. Tree, ORF/segment-aware relationship analysis, and candidate homologous-region exploration are not implemented features in the current `0.1.0-alpha` source.
