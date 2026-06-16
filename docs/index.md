# Gaussian Splatting Tools Comparison

This site documents a structured overview and experimental analysis of Gaussian Splatting training pipelines and XR visualization environments.

---

## What's here

The project is organized around two complementary dimensions:

**[Overview](overview/index.md)** — descriptive documentation of selected Gaussian Splatting tools, covering both commercial and open-source trainers, and a range of visualization and integration tools. No benchmarking — just context.

**[Analysis](analysis/trainers/methodology.md)** — controlled experimental evaluation conducted under fixed, reproducible conditions. Covers open-source training pipelines across indoor and outdoor scenarios, followed by a comparative XR viewer evaluation.

**[How-To](how-to/index.md)** — step-by-step setup and execution guides for each tool included in the experimental analysis.

---

## Reading paths

**"I don't know which trainer to use"**
→ Start with [Which Trainer to Use?](overview/which-trainer.md) — a one-page decision guide with trade-offs and a bottom-line recommendation

**"I want to understand the landscape of tools"**
→ Start with [Overview / Trainers](overview/trainers.md) and [Overview / Viewers](overview/viewers.md)

**"I want to see benchmark results"**
→ Start with [Analysis / Trainer Benchmark Methodology](analysis/trainers/methodology.md), then [Indoor](analysis/trainers/indoor.md), [Outdoor](analysis/trainers/outdoor.md), and [Indoor vs Outdoor](analysis/trainers/indoor-vs-outdoor.md)

**"I want to see the XR viewer evaluation"**
→ Start with [Analysis / XR Viewer Methodology](analysis/viewers/methodology.md), then [XR Viewers](analysis/viewers/xr-viewers.md)

**"I want to reproduce the experiments"**
→ Go directly to [How-To](how-to/index.md)

---

## Experimental workflow

The evaluation follows a structured pipeline:

**Training → Cleaning → Structural Analysis → XR Visualization → Runtime Evaluation**

This separation enables independent assessment of reconstruction structure and density, post-processing impact, immersive rendering stability, and runtime performance behavior.

---

## Methodological scope

The analysis is limited to:

- five selected **open-source training pipelines**
- two **Unity-based XR viewers**

All experiments use fixed datasets, consistent optimization settings, a standardized cleaning workflow, identical hardware, and exported `.ply` files for inspection and XR loading.

Commercial trainers and desktop/web-based viewers are documented at overview level only and are not included in the controlled experimental analysis.

---

## Acknowledgments

Special thanks to **Ernesta Maria Sichetti** for carrying out the experiments, collecting the results, and producing the initial documentation that forms the foundation of this repository.
