# Benchmark Overview

This directory contains the full benchmarking material used to compare multiple open-source Gaussian Splatting implementations under controlled and reproducible conditions.

The benchmarks are organized by scene type (indoor vs outdoor) and include both quantitative evaluations and detailed qualitative visual analyses performed before and after post-processing with SuperSplat.

All experiments were executed on the same hardware platform and using consistent experimental protocols to enable fair cross-method comparisons.

---

## Contents

This folder is structured as follows:

- `indoor.md` — Indoor benchmark, including training statistics, raw and cleaned visual inspections, and scene-cleaning evaluation.
- `outdoor.md` — Outdoor benchmark, including training statistics, raw and cleaned visual inspections, and scene-cleaning evaluation.
- `indoor-vs-outdoor.md` — Cross-scenario analysis highlighting methodological and structural differences between indoor and outdoor reconstructions.

---

## Benchmark Scope

The benchmarks evaluate multiple Gaussian Splatting pipelines under comparable conditions, including:

- fixed datasets per scenario,
- consistent hardware usage,
- uniform visualization and cleaning procedures,
- export to `.ply` format for inspection and post-processing.

Both **raw reconstructions** and **cleaned outputs** are analyzed to assess:

- reconstruction density,
- spatial extent,
- artifact distribution,
- impact of post-processing,
- preservation of structural elements.

---

## Scene Categories

Two scene categories are considered, each represented by a single benchmark scene:

### Indoor

A single confined environment captured in an indoor setting.

See: `indoor.md`.

---

### Outdoor

A single large-scale environment captured in an outdoor setting.

See: `outdoor.md`.

---

## Cross-Scenario Analysis

A dedicated comparison between indoor and outdoor results is provided in: `indoor-vs-outdoor.md`

This section discusses:

- differences in post-processing difficulty,
- spatial extent of raw reconstructions,
- cleaning efficiency,
- background handling strategies,
- densification trends across environments,
- quantitative and qualitative trade-offs.

---

## How to Navigate the Results

If you are interested in:

- indoor reconstructions → see `indoor.md`,
- outdoor reconstructions → see `outdoor.md`,
- methodological differences → see `indoor-vs-outdoor.md`
  
---
