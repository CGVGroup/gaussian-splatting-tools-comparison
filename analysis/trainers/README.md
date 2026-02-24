# Gaussian Splatting Trainers — Technical Benchmark and Visual Analysis

This directory contains the full benchmarking material used to compare multiple open-source Gaussian Splatting implementations under controlled and reproducible conditions.

Benchmarks are organized by **scene type** (indoor vs outdoor) and include both:
- **quantitative evaluations** (model size, Gaussian count, training time), and
- **qualitative visual analyses** performed **before** and **after** post-processing with **SuperSplat**.

All experiments were executed on the same hardware platform and following consistent protocols to enable fair cross-method comparisons.

---

## Contents

- `indoor.md` — Indoor benchmark: training statistics, raw vs cleaned visual inspection, and cleaning impact.
- `outdoor.md` — Outdoor benchmark: training statistics, raw vs cleaned visual inspection, and cleaning impact.
- `indoor-vs-outdoor.md` — Cross-scenario analysis highlighting structural differences between indoor and outdoor reconstructions.

---

## Benchmark Scope

The benchmarks evaluate multiple Gaussian Splatting pipelines under comparable conditions:

- fixed datasets per scenario,
- consistent hardware usage,
- uniform inspection and cleaning workflow in SuperSplat,
- export to `.ply` format for inspection and post-processing.

Both **raw reconstructions** and **cleaned outputs** are analyzed to assess:

- reconstruction density and spatial extent,
- artifact distribution (halos, streaks, floating clusters),
- impact of post-processing on size and structure,
- preservation of relevant structural elements.

---

## Implementations Evaluated

The following open-source pipelines are benchmarked in both scenarios:

- Inria gaussian-splatting
- gsplat
- OpenSplat
- Nerfstudio
- LichtFeld Studio (MCMC densification enabled)

Run instructions are linked in the tables inside `indoor.md` and `outdoor.md`.

---

## Experimental Protocol

All methods were trained with:

- **30,000 optimization iterations**
- the same **151-frame** image set per scenario
- training executed on a **single NVIDIA RTX 4060 GPU**
- default hyperparameters 
- exported reconstructions converted to **`.ply`** for inspection and downstream processing

### Timing and Size Conventions

- **Training time** refers only to the pipeline’s automatic training phase.
- **Output size (MB)** refers to the exported `.ply` file size.

---

## SuperSplat Inspection and Cleaning Workflow

All reconstructions follow a consistent three-stage qualitative workflow.

### Raw inspection

The exported `.ply` is opened in **SuperSplat** to assess:
- compactness of the reconstructed volume,
- outlier/halo distribution,
- detached clusters and far-field artifacts,
- streak / spike-like structures.

### Cleaning

The scene is cleaned in SuperSplat using a consistent set of principles:
- spatial restriction (when feasible),
- distance pruning (conservative outdoors),
- opacity filtering,
- scale-based filtering (x/y/z),
- surface-area filtering,
- manual refinement to preserve meaningful geometry,  
then re-exported as a cleaned `.ply`.

### Post-cleaning inspection

This stage focuses exclusively on the appearance of the cleaned models, highlighting:

- changes in spatial compactness,
- removal of peripheral noise and floating clusters,
- persistence of residual streak artifacts,
- preservation of structural and environmental elements.

Each benchmark file includes screenshots and orbit videos captured in SuperSplat for both raw and cleaned reconstructions.

---

## Scene Categories

Two scene categories are considered, each represented by a single benchmark scene:

### Indoor

A single confined environment captured in an indoor setting.  
See: `indoor.md`.

### Outdoor

A single large-scale environment captured in an outdoor setting.  
See: `outdoor.md`.

---

## Cross-Scenario Analysis

A dedicated comparison between indoor and outdoor results is provided in: `indoor-vs-outdoor.md`.

This section discusses:

- differences in post-processing difficulty,
- spatial extent of raw reconstructions,
- cleaning efficiency and pruning limits,
- quantitative and qualitative trade-offs.

---

## How to Navigate the Results

- Indoor reconstructions → `indoor.md`
- Outdoor reconstructions → `outdoor.md`
- Cross-scenario comparison → `indoor-vs-outdoor.md`

---
