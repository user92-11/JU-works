# Alpha Limitations and Cautions

JU SeqWorkbench Alpha is being developed as a lightweight desktop workbench for practical Sanger/FASTA/MSA sequence inspection, editing, grouping, site/region comparison, and export.

It is an alpha-stage scientific software project. Important results should be checked with representative data, and important source files should be backed up before editing or project operations.

## Scope limitations

JU SeqWorkbench is **not** intended to replace full-scale NGS or genome analysis pipelines.

The current alpha is not designed for:

- raw NGS read processing
- FASTQ quality-control pipelines
- read mapping
- variant calling from mapped reads
- de novo genome assembly
- clinical diagnostic use
- full professional phylogenetic inference pipelines
- full plasmid cloning simulation
- automatic primer-design workflows

The main focus remains post-alignment inspection and editing of small-to-medium Sanger/FASTA/MSA datasets, followed by site/region-oriented comparison, grouping, visualization, and export.

## Dataset size and performance

The current viewer is optimized for practical small-to-medium sequence sets rather than very large alignments.

- Very large datasets may become slow to render or edit.
- Residue coloring and visualization can add rendering overhead on larger alignments.
- Further rendering optimization and custom/virtualized drawing are planned for later development.

## Sanger AB1 limitations

The current Sanger workflow focuses on reviewing an existing basecalled sequence together with its chromatogram trace.

The alpha does not currently provide:

- new basecalling from raw trace data
- automatic forward/reverse consensus building
- automatic mixed-peak or heterozygous-site detection
- advanced automatic quality trimming
- batch Sanger QC reports

Chromatogram navigation is currently matplotlib-based and may not feel fully real-time with very large traces.

## External alignment tools

MAFFT and Clustal Omega are treated as external, user-installed tools.

- External alignment executables are not bundled with JU SeqWorkbench Alpha.
- Users configure a separately installed executable when needed.
- JU SeqWorkbench does not download or redistribute those external aligner binaries.

Availability and behavior therefore depend partly on the user's local tool installation.

## Duplicate Viewer

Duplicate Viewer is an independent editable snapshot intended for inspection and comparison.

- It is not live-synchronized with the original viewer.
- Undo/Redo history is not shared with the original viewer.
- Some project-level save/export operations are intentionally restricted in Duplicate Viewer to reduce accidental overwrites or confusion.

## Undo history and project files

Project files preserve the supported project state, but Undo/Redo history is not stored as part of the project file.

After reopening a saved project, the previous editing history cannot be restored through Undo.

## Alpha-stage analysis caution

JU SeqWorkbench is still undergoing validation with synthetic and representative datasets before public alpha distribution.

During alpha testing:

- numerical and visual outputs should be cross-checked for important analyses
- users should keep backup copies of original FASTA, AB1/ABI, and project files
- the only copy of important or unpublished sequence data should not be used as an editable alpha-test working file

## Deferred or future areas

The following are outside the current alpha scope or remain future work:

- annotation layer
- advanced tree inference
- BLAST/homology-search integration
- structure-linked mutation visualization
- Group Contrast / signature-site discovery
- true split-view editing
- large-dataset rendering redesign
- advanced report generation

These items may be explored later, but they are not required for the initial lightweight sequence-inspection workflow.

## Data and privacy

JU SeqWorkbench Alpha is designed as a local desktop application. Sequence and project files are processed locally unless the user separately uploads or shares data outside the application.

For public GitHub feedback, do not upload confidential or unpublished sequence data to Issues or Discussions.
