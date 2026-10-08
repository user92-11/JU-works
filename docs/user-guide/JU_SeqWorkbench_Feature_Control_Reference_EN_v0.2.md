# JU SeqWorkbench Alpha — Detailed Feature & Control Reference

Version: v0.2  
Target: JU SeqWorkbench Alpha 0.1.x  
Purpose: a practical reference for buttons, checkboxes, input fields, and analysis options that are only summarized in the main User Guide.

> This is an operational reference, not a guarantee of biological interpretation. Verify important results against the original data and appropriate independent methods.

---

## 1. How to use this document

- **User Guide**: learn the overall JU SeqWorkbench workflow.
- **This reference**: look up what an individual control actually does.
- UI labels may appear in English or Korean depending on the selected language; the underlying behavior is the same.

Covered here:

1. ID/name Grouping
2. AA Marker Classification
3. Similarity Clustering
4. Point Visualization
5. Region Visualization
6. Sanger AB1 workflow
7. External MSA
8. Single Sequence Editor

---

# 2. ID/name Grouping

Groups sequences by patterns found in the sequence ID and description/header text.

## 2.1 Ruleset Editor

| Control | Behavior |
|---|---|
| **Ruleset** | Selects the current grouping ruleset. |
| **New** | Creates a new user ruleset. |
| **Save** | Saves the current ruleset and its group/pattern definitions. |
| **Delete (user only)** | Deletes a user-created ruleset. |
| **Copy...** | Copies the current ruleset to a new user ruleset. |
| **Rename...** | Renames a user ruleset. |
| **Default group (unmatched)** | Group used when no pattern matches. Default: `Unclassified`. |
| **+ Group** | Adds a group rule. |
| **- Delete** | Removes the selected group rule. |
| **Copy** | Copies the selected group rule. |
| **Selected group name** | Edits the selected group name. |
| **Patterns** | One pattern per line. Normal matching is substring matching against ID/header text. |

### Pattern matching basics

The matching text combines the sequence ID and description/header.

By default, patterns are matched as **substrings**. For example, `H5N1` can match `sample_H5N1_2024`.

Boundary options are available to reduce accidental matches from short codes.

## 2.2 Grouping options

| Option | Default | Behavior |
|---|---:|---|
| **Case-sensitive matching** | ON | Treats upper- and lowercase characters as different. When OFF, matching and Include/Exclude filtering are case-insensitive. |
| **Show result table** | OFF | Opens a grouping result table. |
| **Save grouped FASTA files** | ON | Saves FASTA files for the resulting groups. |
| **Show/save all overlapping rules (multi-match)** | OFF | OFF uses the first matching rule. ON allows one sequence to belong to multiple groups. |
| **Matched display** | `group:pattern` | Shows `Group:Pattern` or only `Pattern` in the Matched result column. |
| **Remove duplicate Matched values** | ON | Removes duplicate Matched strings in multi-match output. |
| **Prevent overmatching short abbreviations** | ON | Smart boundary matching. Short 1–3 character alphanumeric codes and 1–4 digit numeric codes are treated as boundary-delimited tokens. |
| **Match every pattern as a whole token** | OFF | Forces boundary matching for every pattern. Enabling it disables Smart boundary. |
| **Include** | blank | Before classification, keeps only records whose ID/header contains this substring. |
| **Exclude** | blank | Before classification, removes records whose ID/header contains this substring. |

### Smart boundary example

With Smart boundary enabled and the pattern `HA`:

- `sample_HA_01` → matches
- `xHAy` → does not match

Underscore (`_`) is treated as a boundary because it is not alphanumeric.

If you want to match a pattern even when it is directly attached to other letters or digits, **turn off both boundary-related options**.

For example, with the pattern `CPA` and the ID `14CPA`:

- **Prevent overmatching short abbreviations = ON** → does not match because the preceding `4` is alphanumeric, so `CPA` is not treated as a separate token
- **Match every pattern as a whole token = ON** → does not match because a boundary is required around the pattern
- **Both options OFF** → normal substring matching is used, so `CPA` matches inside `14CPA`

Therefore, to match embedded forms such as `14CPA`, `XCPA`, or `preCPApost`, disable both **Smart boundary** and **whole-token matching**.

### Rule order

With multi-match OFF, the **first matching rule wins**. Put more specific overlapping rules before broader rules.

---

# 3. AA Marker Classification

Classifies sequences using user-defined amino-acid positions and allowed residues.

## 3.1 Ruleset Editor

| Control | Behavior |
|---|---|
| **Ruleset** | Selects the AA marker ruleset. |
| **New / Save / Delete / Copy / Rename** | Creates, saves, deletes user rulesets, copies, or renames them. |
| **Default group** | Group used when no group rule matches. |
| **+ Group / - Group / Copy** | Adds, removes, or copies a group rule. |
| **Import rules** | Imports rules into the current editor; imported rules can replace the current rules or be appended. |
| **Conditions (AND)** | All conditions in a group must match. |
| **Position** | 1-based amino-acid position. |
| **Allowed AA** | One or more accepted amino-acid tokens at that position. |
| **+ Condition / - Condition** | Adds or removes a condition for the selected group. |
| **Selected group name** | Edits the selected group name. |

### Allowed AA examples

Supported examples include:

- `D`
- `DE`
- `D,E`
- `D E`
- `D|E`
- `*`
- `-X`

Commas, spaces, semicolons, `/`, and `|` can separate entries. `X`, `*`, and `-` are accepted marker tokens.

### Condition relationship

Multiple conditions within one group use **AND** logic. A sequence must satisfy every condition in that group.

## 3.2 Run options

| Option | Default | Behavior |
|---|---:|---|
| **Filter** | blank | Keeps records whose ID or description/header contains the specified substring. |
| **Stop codon handling** | selected UI value | Controls stop-codon handling while preparing AA analysis sequences. |
| **Truncate at first stop** | available | Uses the translated sequence only up to the first stop. |
| **Keep stop symbols** | available | Keeps `*` in the AA sequence. |
| **Exclude sequences with stop codons** | available | Excludes sequences containing stop codons from classification. |
| **Trim NT length to a multiple of 3** | ON | Trims NT-backed sequences to a length divisible by 3 before translation. |
| **Show/save all matching rules** | OFF | OFF uses the first matching group. ON allows a sequence to be included in all matching groups. |
| **Run** | — | Runs the rules exactly as currently edited, even if the ruleset has not been saved. |
| **Cancel** | — | Closes without running. |

---

# 4. Similarity Clustering

Calculates pairwise sequence similarity and creates clusters at a user-defined threshold.

## 4.1 Comparison settings

| Control | Default | Behavior |
|---|---:|---|
| **NT / AA** | NT | Selects the comparison basis. |
| **Cluster threshold** | 99.0% | Similarity threshold used to connect sequences into clusters. |
| **Full sequence** | ON | Uses the full available comparison sequence. |
| **Manual range** | OFF | Uses a 1-based inclusive range such as `100-500`. The range uses the coordinates of the currently selected NT/AA comparison mode. |
| **Include gaps (`-`)** | OFF | ON includes gap positions; `-` vs `-` counts as a match. OFF skips positions containing gaps. |

### NT ambiguity handling

| Mode | Behavior |
|---|---|
| **Ignore ambiguous positions** | Skips positions containing ambiguous IUPAC bases. |
| **Compatible IUPAC match** | Counts a match when the possible-base sets overlap. |
| **Strict exact match** | Requires exact character equality. |

### AA ambiguity handling

| Mode | Behavior |
|---|---|
| **Ignore X/*** | Skips positions containing X or `*`. |
| **Strict exact match** | Requires exact character equality. |

## 4.2 Result display

| Option | Default | Behavior |
|---|---:|---|
| **Show variation range summary table and highlight on click** | ON | Shows summarized variable ranges and allows result-driven highlighting in the viewer. |
| **Show similarity heatmap** | OFF | Displays the similarity matrix as a heatmap. |
| **Reorder heatmap by cluster order** | ON when heatmap is enabled | Reorders heatmap rows/columns by cluster order. |
| **Show dendrogram (average linkage)** | OFF | Displays an average-linkage dendrogram. Large datasets may trigger limits or warnings. |
| **Auto-switch display mode when needed** | ON | Helps align viewer display mode with the comparison mode for highlighting. It does not change the similarity calculation itself. |

## 4.3 Export

| Option | Default | Behavior |
|---|---:|---|
| **Save cluster FASTA files** | OFF | Saves cluster FASTA files. |
| **Export result bundle to a folder** | OFF | Saves related CSV/FASTA outputs as a folder bundle. |
| **Save similarity matrix CSV** | OFF | Saves the complete similarity matrix as CSV. Enabled with result-bundle export; large datasets can produce very large files. |

---

# 5. Point Visualization

Analyzes selected known positions/sites in AA, NT, or Codon coordinates.

## 5.1 Input and analysis controls

| Control | Default | Behavior |
|---|---:|---|
| **Preset** | Direct input | Loads a saved site preset. |
| **Save / Overwrite / Delete** | — | Saves a new preset, overwrites the selected user preset, or deletes a user preset. |
| **Unit** | determined by menu | Fixed by the Point Visualization entry used: AA, NT, or Codon. |
| **Positions** | — | AA/NT examples: `10,25,50`. Codon examples: `1-3,15-17` or `[1,2,3],[15,16,17]`. Positions are 1-based. |
| **Stop codon handling** | AA only | Controls stop handling during AA preparation. |
| **Include reference sequence** | OFF | Includes the reference itself in count/detail samples. With OFF, the reference remains the comparison reference but is excluded from sample counting. |
| **Create per-strain detail table** | OFF | Builds per-strain, per-position detail output. This can substantially increase result size. |
| **Trim length to a multiple of 3** | ON in AA | Trims NT-backed input to a multiple of 3 before AA preparation. Hidden in NT/Codon Point modes. |
| **Filter** | blank | Keeps records whose ID or description contains the substring. Current behavior is simple case-sensitive substring matching. |
| **Reference** | Automatic selection | Selects the comparison reference. Automatic selection uses available/selected sequences. |
| **Convert viewer display to the analysis basis** | OFF | Allows the viewer display to be converted to the analysis basis when useful for linking/highlighting. |
| **Run / Run again** | — | Runs the analysis. |

> `Include reference sequence` controls whether the reference participates in the current analysis sample. It is not the same as a future option to draw a dedicated reference track in exported figures.

## 5.2 Visualization tab

Available plot types depend on mode/data and include:

- Logo plot
- Codon logo plot
- Heatmap (Position × residue/token)
- Binary mutation map
- Categorical mutation map
- Entropy
- Major allele frequency
- Excluded count
- Total-variant bar
- Composition stacked bar

| Control | Default | Behavior |
|---|---:|---|
| **Bits (information)** | ON | Uses information-content bits for logo-style output. |
| **Include `-` / `*` / `X`** | ON | Includes gap, stop, and unknown AA tokens where relevant. |
| **Title / X label / Y label / X ticks / Y ticks** | ON | Shows or hides each figure element. |
| **Scale** | 100% | Adjusts figure scale. |
| **Text** | 10 pt | Adjusts figure text size. |
| **Font** | environment-dependent | Selects from common fonts plus detected supported Korean fonts. |
| **Style: Default** | default | General display/export style. |
| **Style: Publication** | available | Uses a larger figure, readable labels, white background, and 600-DPI raster export settings. |
| **Render again** | — | Re-renders with the current visualization/style settings. |
| **Save figure...** | — | Saves the current figure. |

## 5.3 Counts and Detail tabs

| Control | Behavior |
|---|---|
| **Filter** | Filters displayed result rows across table content. |
| **Sort: Input / Ascending / Descending** | Selects result ordering. |
| **Export Counts CSV** | Saves the Counts table. |
| **Detail: Individual strains** | Shows one row per strain/item. |
| **Detail: Group by variant** | Groups identical variants and lists IDs together. |
| **Export Detail CSV** | Saves the Detail table. |

---

# 6. Region Visualization

Analyzes one or more continuous regions in AA, NT, or Codon units.

## 6.1 Basic controls

| Control | Behavior |
|---|---|
| **Preset / Save / Overwrite / Delete** | Loads or manages region presets. |
| **Unit** | Selects `AA`, `NT`, or `Codon`. |
| **Regions** | Accepts multiple ranges, e.g. `10-50,120-180`, `10-50;120-180`, or labeled ranges such as `HA1:10-50,HA2:60-120`. |
| **Layout: Panel** | Draws regions as separate panels. |
| **Layout: Concatenate** | Draws selected regions on a concatenated axis. |
| **Layout: Summary** | Emphasizes region-level summary output. |
| **Reference** | Selects the reference for reference-based metrics. Entropy itself is not a reference-relative metric. |

## 6.2 Metrics

| Metric | Meaning |
|---|---|
| **Entropy** | Token diversity at each position/codon; not calculated relative to a reference strain. |
| **Valid sample size** | Number of usable samples at each position. |
| **Excluded count** | Number of samples excluded from calculation at each position. |
| **Gap rate** | Gap frequency/rate. |
| **Unknown rate** | Unknown-token rate; AA uses X and NT/Codon uses unknown/ambiguous base handling. |
| **Non-ref burden** | Summarizes difference burden relative to the selected reference. |
| **Mutation map (binary)** | Strain × position binary map of reference match/difference. |
| **Strain-wise Region Profile** | Draws region-level patterns for individual strains. |

## 6.3 Strain-wise Region Profile controls

| Control | Default | Behavior |
|---|---:|---|
| **Binary non-ref** | available | Shows reference match/difference across the region. |
| **Local non-ref density** | available | Calculates local non-reference density using a moving window. |
| **Burden summary** | available | Summarizes per-strain region burden. |
| **Group mean profile** | disabled in Alpha | Reserved for future explicit grouping/metadata sources. |
| **Average range (positions)** | 20 | Moving-window size. Smaller values preserve sharper changes; larger values smooth the profile. |
| **Fix y-axis to 0-1** | OFF | Fixes local-density Y range to 0–1 instead of auto-scaling. |
| **Mean line / Median line / Off** | Mean line | Selects the summary line over individual local-density profiles. |
| **Top 20 / Top 50 / All** | Top 50 | Number of strains displayed in burden summary. |
| **Group source** | disabled | No explicit grouping-metadata source is available in Alpha. |

## 6.4 Variable-position labels

| Control | Default | Behavior |
|---|---:|---|
| **Change label threshold** | 0.5 | Entropy threshold used to select additional X-axis labels. |
| **Show variable position labels** | ON | Shows extra labels for eligible variable positions/chunks. |
| **Variable label mode: Clustered** | default | Reduces crowding by clustering labels. |
| **Top N / Threshold / All (debug) / Off** | available | Controls how many eligible text labels are drawn. |
| **Color X-axis annotation labels by region** | OFF | Applies muted region colors to position/annotation labels. |

## 6.5 Figure options

| Control | Default | Behavior |
|---|---:|---|
| **Include gap / unknown / stop** | metric-dependent | Includes those token categories where the selected metric supports them. |
| **Title / X label / Y label / X ticks / Y ticks** | ON | Shows/hides figure elements. |
| **Scale** | 100% | Adjusts figure size. |
| **Font size / Font** | 10 pt / environment-dependent | Controls figure typography. |
| **Style: Default / Publication** | Default | Chooses the figure style. |
| **Show metrics table** | enabled after analysis | Opens the computed metric table. |
| **Run / Re-render** | — | Runs the analysis or re-renders compatible cached results. |
| **Save figure** | — | Exports supported raster/vector formats such as PNG/JPEG or SVG/PDF. |

---

# 7. Sanger AB1 Workflow

## 7.1 Import options

| Control | Behavior |
|---|---|
| **Show trace window** | Opens the chromatogram trace window after import. |
| **Original orientation** | Imports the basecalled sequence in its original orientation. |
| **Reverse complement** | Imports the reverse-complement orientation. |

## 7.2 Trace viewer

| Control | Behavior |
|---|---|
| **Orientation** | Switches trace/basecall display between original and reverse complement. |
| **Previous / Next** | Moves the displayed base region backward/forward. |
| **Base position** | Shows/controls the current base range. |
| **Zoom in / Zoom out / Reset zoom** | Changes chromatogram zoom. |
| **Trim start / Trim end** | Defines the basecall interval to use. |
| **Import original** | Imports the current read in original orientation. |
| **Import reverse complement** | Imports it in reverse-complement orientation. |

`Open AB1` creates an AB1-oriented viewer/trace workflow, while `Import` appends the selected sequence to the current viewer.

---

# 8. External MSA

JU SeqWorkbench Alpha does not bundle or automatically download external aligner binaries. **The currently supported Alpha execution path is a separately installed MAFFT executable.** Clustal Omega configuration/runner code is retained, but its Windows Alpha workflow has not been validated and its UI controls are disabled in this release.

## 8.1 Configure external aligners

| Control | Behavior |
|---|---|
| **MAFFT path** | Specifies `mafft.bat` or another valid MAFFT executable path. |
| **Choose mafft.bat...** | Browses for the MAFFT executable. |
| **Auto-detect MAFFT** | Checks the configured path/PATH and supported common executable names. |
| **Test MAFFT** | Runs a small temporary FASTA alignment to verify the executable. |
| **Open MAFFT download page** | Opens the official MAFFT download page. |
| **Clustal Omega controls** | Disabled in the current Alpha. They are not a supported execution path for this release even if Clustal Omega is installed separately. |
| **Clear paths** | Clears path fields; use Save to persist the change. |
| **Save / Cancel** | Saves or discards the path settings. |

## 8.2 Run external MSA

| Control | Behavior |
|---|---|
| **Aligner: MAFFT** | The supported external aligner in the current Alpha. |
| **Clustal Omega** | Disabled in the current Alpha UI. |
| **Run** | Writes temporary FASTA input and launches MAFFT. |
| **Cancel** | Closes without running. |

## 8.3 MSA completed

| Control | Behavior |
|---|---|
| **Open aligned result in new viewer/window** | Keeps the current viewer and opens the alignment in a new viewer. |
| **Replace current viewer** | Replaces the current working sequence set with the aligned result. |
| **Also save aligned FASTA** | Saves aligned FASTA together with the selected viewer action. |
| **Save now...** | Saves aligned FASTA immediately. |
| **Show log** | Opens execution/diagnostic logs. |
| **Cancel** | Closes without applying the aligned result. |

---

# 9. Single Sequence Editor

Provides a focused editor for one sequence.

| Control | Default | Behavior |
|---|---:|---|
| **ID** | current ID | Edits the sequence ID; Apply validates the rename and uniqueness. |
| **Kind** | current kind | Displays the sequence type. |
| **Length** | current length | Displays the compact sequence length, excluding display labels/spacing. |
| **Font** | 11 | Editor font size. |
| **Line width** | 80 | Residues displayed per line; range 15–120. |
| **Group: None / 3 / 5 / 10** | 10 | Adds display-only spacing between residue groups. The spaces are not stored in the sequence. |
| **Overwrite** | OFF | Enables overwrite behavior at the cursor instead of insertion. |
| **Apply** | — | Applies changes back to the parent viewer through the supported edit/undo path. |
| **Apply and Close** | — | Applies and closes. |
| **Close** | — | Closes without applying. |

Input characters are validated according to the current NT/AA kind. Display line numbers and spacing are removed before storing the sequence.

---

# 10. Frequently confused behaviors

### Does selecting a reference automatically include it in sample counts?
Not always. In Point Visualization, **Include reference sequence** separately controls whether the reference is included in the analysis sample.

### Is `_` treated as a token boundary in ID grouping?
Yes for boundary matching. `_` is non-alphanumeric, so it acts as a boundary.

### What happens when multi-match is disabled?
The **first matching rule** is used, so rule order can affect the result.

### Are AA Marker positions zero-based?
No. User-entered marker positions are **1-based**.

### Does Similarity Manual range convert NT coordinates to AA coordinates automatically?
No. The range is interpreted in the currently selected comparison mode's coordinates.

### Are MAFFT or Clustal Omega included in JU SeqWorkbench Alpha?
No external aligner binary is included. The current Alpha connects to a separately installed **MAFFT** executable; the Clustal Omega path is disabled in this release.

---

# 11. Related documents

- [JU SeqWorkbench Alpha User Guide — English](user_guide_en.md)
- [Quick Start PDF](JU_SeqWorkbench_Quick_Start_EN.pdf)
- [Alpha Limitations & Cautions](../limitations/index.md)
