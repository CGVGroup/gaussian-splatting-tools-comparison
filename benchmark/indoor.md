# Indoor Dataset — Benchmark Results

This section reports quantitative benchmarking results for several open-source Gaussian Splatting implementations evaluated on the same indoor dataset.

---

## Dataset Description

The indoor dataset consists of **151 frames** extracted from the following video sequence:

https://huggingface.co/datasets/DL3DV/DL3DV-10K-Sample/tree/main/5c3af581028068a3c402c7cbe16ecf9471ddf2897c34ab634b7b1b6cf81aba00

---

## Experimental Protocol

All implementations were trained under the following conditions:

- **30,000 optimization iterations**
- Same image set (151 frames)
- Training executed on a **single NVIDIA RTX 4060 GPU**
- Default hyper-parameters were used unless explicitly modified for reproducibility
- Exported models were converted to `.ply` format 

For LichtFeld Studio, the MCMC densification pipeline was enabled.

---

## Quantitative Results — Indoor Dataset

| Tool | Output Size (MB) | # Gaussians | MB / 100k | Training Time (min) | Min / 100k | Densification Strategy | Discussion |
|------|----------------:|------------:|----------:|-------------------:|-----------:|----------------------|------------|
| Inria GS | 231 | 867,000 | 26.6 | 150 | 17.3 | Adaptive density control | [How-To](../tools/inria.md) |
| gsplat | 230 | 1,000,000 | 23.0 | 50 | 5.0 | CUDA-optimized default | [How-To](../tools/gsplat.md) |
| OpenSplat | 123 | 510,000 | 24.1 | 60 | 11.7 | Native pruning | [How-To](../tools/opensplat.md) |
| Nerfstudio | 43 | 170,000 | 25.3 | 30 | 17.6 | Adaptive culling + gsplat backend | [How-To](../tools/nerfstudio.md) |
| LichtFeld Studio | 242 | 1,000,000 | 24.2 | 64 | 6.4 | MCMC pipeline | [How-To](../tools/lichtfeld.md) |

---

## Observations

- **Nerfstudio** produced the most compact representation and shortest training time, at the cost of reduced Gaussian density.

- **OpenSplat** achieved a favorable compromise between output size and visual quality through aggressive pruning.

- **gsplat** and **LichtFeld Studio** generated the densest models, with correspondingly larger output files.

- The original **Inria GS** reference implementation remained the slowest, although it produced structurally stable reconstructions.

---
