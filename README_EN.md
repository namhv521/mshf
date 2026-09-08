<div align="center">

# MSHF — Multi-Scale Hybrid Fusion

**Vietnamese Legal Document Tampering Detection and Localization**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![GitHub Release](https://img.shields.io/github/v/release/Susois/mshf)](https://github.com/Susois/mshf/releases/latest)

[Tiếng Việt](README.md)

</div>

---

## Overview

MSHF is a PDF document tampering detection system for Vietnamese legal texts, built on a **Multi-Scale Hybrid Fusion** architecture that combines three independent feature branches:

- **Branch A** — Semantic: CER/WER + PhoBERT + LayoutLMv3 *(8 features)*
- **Branch B1** — Line-level: insert/delete/modify counts and ratios *(16 features)*
- **Branch C** — Geometric: delta font/spacing/margin + positional residual *(16 features)*

All branches feed into an **XGBoost** classifier with 5 output labels:
`original` · `insert` · `delete` · `modify` · `layout`

The system also localizes tampered lines on each page and generates an interactive HTML report with color-coded highlights.

---

## What the System Does

MSHF takes a reference PDF and a document to inspect, then:

- Compares their text content and page layout.
- Classifies the document as `original`, `insert`, `delete`, `modify`, or `layout`.
- Distinguishes inserted, deleted, and modified lines and localizes them page by page.
- Combines semantic, line-level, and geometric signals instead of relying on a single type of evidence.
- Produces predictions, confidence scores, and an HTML report with visually highlighted suspicious regions.

The system is designed for Vietnamese legal documents, where small changes in wording, punctuation, spacing, or positioning can be meaningful.

## Repository Scope

This repository covers the full workflow: extracting information from PDFs, aligning corresponding lines, building features, training models, evaluating results, and localizing tampering. It also includes utilities for dataset construction, data-quality auditing, robustness evaluation, and experiments on different document splits.

The three feature branches complement one another:

- **Semantic** captures differences in text content and meaning.
- **Line-level** models insertion, deletion, and modification operations.
- **Geometric** detects anomalies in fonts, spacing, margins, and page position.

---

## Results

| Task | Configuration | Macro F1 | Balanced Acc |
|---|---|---|---|
| Binary (tampered/authentic) | A+B1+C / single | **98.86%** | 99.41% |
| 5-class | A+B1+C / stacking | **98.52%** | 98.52% |

Evaluated on the **VEDTD** dataset (1,490 Vietnamese legal documents, 5-fold source-disjoint cross-validation).

---

## System Requirements

| Component | Minimum | Recommended |
|---|---|---|
| Python | 3.10 | 3.11+ |
| RAM | 8 GB | 16 GB |
| Disk | 1 GB | 2 GB |
| GPU | — | CUDA-compatible (speeds up Branch A) |

---

## License

This project is licensed under the [MIT License](LICENSE).  
See [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) for third-party library attributions.
