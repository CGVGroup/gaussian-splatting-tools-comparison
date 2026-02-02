# Outdoor Dataset — Benchmark Results

This section reports quantitative benchmarking results for several open-source Gaussian Splatting implementations evaluated on the same outdoor dataset.

---

## Dataset Description

The outdoor dataset consists of **151 frames** extracted from the followingvideo sequence:

https://huggingface.co/datasets/DL3DV/DL3DV-10K-Sample/tree/main/ba55c875d20c34ee85ffc72264c4d77710852e5fb7d9ce4b9c26a8442850e98f

---

## Experimental Protocol

All implementations were trained under the following conditions:

- **30,000 optimization iterations**
- Same image set (151 frames)
- Training executed on a **single NVIDIA RTX 4060 GPU**
- Default hyper-parameters were used unless explicitly modified for reproducibility
- Exported models were converted to `.ply` format 

For LichtFeld Studio, the **MCMC densification pipeline** was enabled.

---

## Quantitative Results — Outdoor Dataset

| Tool | Output Size (MB) | # Gaussians | MB / 100k | Training Time (min) | Min / 100k | Densification Strategy | Discussion |
|------|----------------:|------------:|----------:|-------------------:|-----------:|----------------------|------------|
| Inria GS | 188 | 777,067 | 24.2 | 60 | 7.7 | Adaptive density control | [How-To](../tools/inria.md) |
| gsplat | 238 | 1,031,707 | 23.0 | 45 | 4.4 | CUDA-optimized default | [How-To](../tools/gsplat.md) |
| OpenSplat | 143 | 589,291 | 24.3 | 50 | 8.5 | Native pruning | [How-To](../tools/opensplat.md) |
| Nerfstudio | 48 | 19,754 | 243.1 | 25 | 126.6 | Adaptive culling + gsplat backend | [How-To](../tools/nerfstudio.md) |
| LichtFeld Studio | 242 | 1,000,000 | 24.2 | 50 | 5.0 | MCMC pipeline | [How-To](../tools/lichtfeld.md) |

---

## Observations

- **Nerfstudio** produced an extremely compact model in terms of Gaussian count, resulting in the smallest output file and fastest training time among all evaluated tools.

- **OpenSplat** achieved a favorable compromise between reconstruction density and output size through aggressive pruning.

- **gsplat** and **LichtFeld Studio** generated the densest reconstructions, with correspondingly larger output files.

- The original **Inria GS** reference implementation remained slower than the other pipelines, although it produced stable large-scale outdoor reconstructions.

---

