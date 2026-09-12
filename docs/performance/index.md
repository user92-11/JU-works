# JU SeqWorkbench Performance Baseline

This page records development-time performance baselines for JU SeqWorkbench so that later renderer changes can be measured against a fixed reference point.

> **Scope:** this is a JU-only development baseline. It is **not** a cross-software ranking or a claim that these timings represent every real-world workflow.

## Why publish a baseline?

The current sequence renderer still uses a full-document `QPlainTextEdit + QSyntaxHighlighter` architecture. Before migrating or substantially changing that renderer, a fixed baseline was captured so that later optimization work can be evaluated with the same fixtures and metrics.

The public dataset intentionally contains **only JU SeqWorkbench measurements**. Internal observations involving other software are kept separate because the measurement methods are not directly comparable enough for a fair public benchmark.

## Baseline snapshot

- **Measurement date:** 2026-09-08
- **Frozen/public baseline:** 2026-09-12
- **Development phase:** `post-safe-optimization / pre-renderer-migration`
- **Build:** `beta-dev@96ae3f9`
- **Renderer architecture:** `QPlainTextEdit + QSyntaxHighlighter` full-document architecture
- **Platform:** native Windows Qt
- **Visible window:** 1280 × 800
- **Python:** 3.13.5
- **Qt:** 6.9.1
- **Repetitions:** 3 per metric
- **Reported value:** median; raw CSV also preserves minimum and maximum

Lower timing values are better. All timing values below are milliseconds.

## Test fixtures

| Fixture | Rows | Alignment columns | Total cells |
|---|---:|---:|---:|
| 35 × 1,000 | 35 | 1,000 | 35,000 |
| 35 × 5,000 | 35 | 5,000 | 175,000 |
| 100 × 5,000 | 100 | 5,000 | 500,000 |
| 100 × 10,000 | 100 | 10,000 | 1,000,000 |
| 500 × 1,000 | 500 | 1,000 | 500,000 |

The raw dataset includes fixture SHA-256 values so the same synthetic inputs can be tracked consistently.

## Selected median timings

| Fixture | Refresh | Edit + refresh | Color ON | Dot ON |
|---|---:|---:|---:|---:|
| 35 × 1,000 (35k cells) | 167.7 ms | 135.9 ms | 659.5 ms | 138.9 ms |
| 35 × 5,000 (175k cells) | 674.1 ms | 618.8 ms | 1,581.6 ms | 328.9 ms |
| 100 × 5,000 (500k cells) | 772.5 ms | 781.0 ms | 4,775.9 ms | 916.6 ms |
| 100 × 10,000 (1.0M cells) | 1,452.3 ms | 1,731.6 ms | 9,545.6 ms | 1,912.6 ms |
| 500 × 1,000 (500k cells) | 213.0 ms | 185.2 ms | 3,182.4 ms | 301.9 ms |

| Fixture | H-scroll | V-scroll | Color OFF | 10-column highlight |
|---|---:|---:|---:|---:|
| 35 × 1,000 (35k cells) | 53.6 ms | 0.0 ms* | 97.8 ms | 84.0 ms |
| 35 × 5,000 (175k cells) | 108.6 ms | 0.0 ms* | 189.2 ms | 135.4 ms |
| 100 × 5,000 (500k cells) | 206.2 ms | 194.5 ms | 370.4 ms | 295.0 ms |
| 100 × 10,000 (1.0M cells) | 398.7 ms | 387.0 ms | 816.0 ms | 503.6 ms |
| 500 × 1,000 (500k cells) | 53.5 ms | 102.4 ms | 104.1 ms | 333.9 ms |

\* The 35-row fixtures did not overflow vertically in the 800-pixel test window, so vertical scrolling was effectively a no-op and should not be interpreted as a meaningful scrolling result.

## What the baseline already shows

The largest bottleneck in this renderer is residue coloring with **Color ON**. Under the current full-document highlighting architecture, the 100 × 10,000 fixture reached a median of about **9.55 s** for that metric. This is one of the reasons renderer migration and visible-range/incremental rendering are post-alpha priorities.

The two 500,000-cell fixtures also behave differently depending on whether the dataset is wider or taller. That is useful for future optimization work because total cell count alone does not fully describe renderer cost.

This baseline is deliberately published **before** the renderer is improved. Its purpose is to preserve an honest reference point for later before/after development logs rather than to present tuned marketing numbers.

## Interpretation limits

- These measurements come from the JU internal native timing harness and the stated test conditions.
- They should be compared only with later JU measurements using the same fixtures, metrics, and sufficiently similar conditions.
- They are not equivalent to subjective user-perceived responsiveness or end-to-end file-open time.
- No results from other applications are included in this public baseline.
- Future renderer work may change the architecture substantially; when that happens, the comparison will be reported as a development before/after result rather than as a universal performance claim.

## Raw public data

[Download/view the public JU pre-renderer baseline CSV](../../benchmarks/renderer_baseline/ju_pre_renderer_baseline_2026-09-12.csv)

The CSV contains all 40 measurements: 5 fixtures × 8 metrics, with median/min/max values and measurement metadata.

---

[Back to JU SeqWorkbench Alpha](../../)
