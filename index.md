# Sequence Viewer Development Log

A desktop sequence inspection and visualization tool for practical Sanger/FASTA-based workflows.

Sanger/FASTA 서열 데이터를 열고, 편집하고, 정렬 결과를 확인한 뒤, grouping·typing·site/region visualization·table/figure export까지 빠르게 이어가기 위한 개인 개발 데스크톱 도구입니다.

> Current status: pre-alpha development
> This project is not yet distributed. The current focus is feature stabilization, packaging review, license review, and development documentation.

---

## Project overview

This project started from a practical problem: routine sequence inspection often requires moving between several tools for sequence viewing, manual checking, alignment review, mutation inspection, grouping, and figure/table preparation.

The goal of this viewer is not to replace every existing bioinformatics tool. Instead, it aims to make common inspection workflows faster and more connected.

The current development focus is:

* opening and reviewing FASTA sequence files
* opening and importing Sanger AB1 reads
* checking basecalled sequences and chromatogram traces
* editing and organizing sequence data
* reviewing alignment results
* grouping sequences by ID labels, marker rules, or similarity
* inspecting selected mutation sites
* visualizing continuous sequence regions
* exporting tables, FASTA files, and figures

---

## Core workflow

The intended workflow is:

```text
FASTA / AB1 input
→ sequence viewing and editing
→ alignment review
→ grouping / typing / similarity inspection
→ site or region visualization
→ table / figure / FASTA export
```

This workflow is designed for practical sequence-checking situations where users need to move quickly from raw sequence data to interpretable tables and figures.

---

## Current feature areas

### Sequence viewer and editing

The viewer supports basic sequence viewing, selection, copy/paste workflows, column-based editing, undo/redo behavior, and single-sequence editing.

This part focuses on making manual sequence inspection less painful while preserving the current working state consistently.

### Sanger AB1 workflow

The AB1 workflow supports opening Sanger trace files, displaying chromatogram traces, importing basecalled sequences, and handling reverse-complement orientation.

This is intended to help users quickly compare raw Sanger reads with the main sequence viewer workflow.

### Grouping and typing

The project includes early workflows for ID/label-based grouping, amino-acid marker-based typing, and similarity-based sequence grouping.

These functions are designed to help users organize sequence sets before deeper inspection or export.

### Visualization and inspection

The visualization workflow is divided into two major types:

* site-based visualization for known marker or hotspot positions
* region-based visualization for continuous intervals or multiple regions

This separation is intentional because selected-position inspection and continuous-region inspection answer different biological questions.

### Export and reporting

Current export workflows focus on practical outputs such as CSV tables, FASTA files, grouped FASTA outputs, and figure exports.

Full report generation is treated as future work.

---

## Development logs

### Foundation

* [Devlog 01 — Project Direction](./devlog/01-project-direction/)
* [Devlog 02 — Data Layer and Working State](./devlog/02-data-layer-and-working-state/)
* [Devlog 03 — Editing and Interaction Model](./devlog/03-editing-and-interaction-model/)

### Analysis and visualization

* [Devlog 04 — Classification and Grouping](./devlog/04-Classification%20and%20grouping/)
* [Devlog 05 — Visualization and Inspection](./devlog/05-visualization-and-inspection/)
* [Devlog 06 — Grouping Rules and Typing Workflow](./devlog/06-grouping-rules-and-typing-workflow/)
* [Devlog 07 — Annotation and Region Metadata](./devlog/07-annotation-and-region-metadata/)

### Output, Sanger, and alpha preparation

* [Devlog 08 — Export and Reporting Workflow](./devlog/08-export-and-reporting-workflow/)
* [Devlog 09 — Sanger AB1 Workflow](./devlog/09-sanger-AB1-workflow/)
* [Devlog 10 — External MSA and Tool Manager](./devlog/10-external-MSA-and-tool-manager/)
* [Devlog 11 — Packaging, License, and Alpha Preparation](./devlog/11-packaging-license-and-alpha-preparation/)

---

## Notes

This project is currently in pre-alpha development.

At this stage:

* no public release package is provided
* no tester recruitment is open
* no sales or pricing information is provided
* external tool packaging and open-source license compliance are still being reviewed
* the current focus is development documentation and workflow stabilization

Screenshots and short workflow demos will be added as the alpha preparation progresses.
 
## Documentation

- [Roadmap](docs/roadmap/)
- [Scope](docs/scope/)
- [Workflow Concept](docs/workflow/)
- [Planned Features](docs/features/)
