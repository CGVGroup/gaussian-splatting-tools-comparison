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

For LichtFeld Studio, the **MCMC densification pipeline** was enabled.

---

## Quantitative Results — Indoor Dataset

| Tool | Output Size (MB) | # Gaussians | MB / 100k | Training Time (min) | Min / 100k | Densification Strategy | Discussion |
|------|----------------:|------------:|----------:|-------------------:|-----------:|----------------------|------------|
| Inria GS | 231 | 867,000 | 26.6 | 150 | 17.3 | Adaptive density control | [How-To](../tools/inria.md) |
| gsplat | 230 | 1,000,000 | 23.0 | 50 | 5.0 | CUDA-optimized default | [How-To](../tools/gsplat.md) |
| OpenSplat | 123 | 510,000 | 24.1 | 60 | 11.7 | Native pruning | [How-To](../tools/opensplat.md) |
| Nerfstudio | 43 | 170,000 | 25.3 | 30 | 17.6 | Adaptive culling + gsplat backend | [How-To](../tools/nerfstudio.md) |
| LichtFeld Studio | 242 | 1,000,000 | 24.2 | 64 | 6.4 | MCMC pipeline | [How-To](../tools/lichtfeld.md) |

## Observations

- **Nerfstudio** produced the most compact representation and shortest training time, at the cost of reduced Gaussian density.

- **OpenSplat** achieved a favorable compromise between output size and visual quality through aggressive pruning.

- **gsplat** and **LichtFeld Studio** generated the densest models, with correspondingly larger output files.

- The original **Inria GS** reference implementation remained the slowest, although it produced structurally stable reconstructions.

---

## Qualitative Evaluation Protocol

Beyond quantitative benchmarking, a qualitative evaluation was conducted on all
reconstructed scenes.

Each raw `.ply` output was first inspected visually using **SuperSplat** in
order to assess noise distribution, structural coherence, and rendering
stability.

Subsequently, a consistent scene-cleaning procedure was applied to all models.
The cleaned reconstructions were then re-inspected under the same conditions to
enable direct visual comparison between raw and post-processed outputs.

This two-stage inspection protocol supports the qualitative analyses and visual
materials presented in the following sections.

## Visual Inspection — Raw Reconstructions (Before Cleaning)

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

Before applying any post-processing or pruning, all reconstructed Gaussian
Splatting models were visually inspected using **SuperSplat Editor**.

The goal of this inspection phase was:

- to evaluate the **spatial compactness** of the reconstruction,
- to analyze the **distribution of outlier Gaussians**,
- to identify large-scale **floating artifacts**,
- and to qualitatively assess differences between densification strategies
  prior to any cleaning operations.

Each tool was evaluated using its exported `.ply` model under identical
visualization conditions.

### Inria Gaussian Splatting — Raw Output

The raw reconstruction produced by Inria shows:

- a well-formed central scene volume,
- recognizable geometry of the shelves and furniture,
- peripheral filament-like structures,
- moderate outliers surrounding the main reconstruction.

## Inria GS — Raw Reconstruction

![](../media/indoor/inria/raw/images/inria_00.png)

![](../media/indoor/inria/raw/images/inria_01.png)


### gsplat — Raw Output

The gsplat reconstruction exhibits:

- a dense core region with good structural detail,
- a wide radial spread of Gaussians,
- long streak-like artifacts extending outward,
- substantial peripheral noise.

### OpenSplat — Raw Output

The OpenSplat model shows:

- a clearly defined central structure,
- outliers distributed mainly near the scene boundary,
- fewer extreme-distance splats compared to gsplat,
- moderate peripheral clutter.

### Nerfstudio — Raw Output

Nerfstudio’s raw model presents:

- a relatively small and sparse central cluster,
- several isolated Gaussian groups far from the main scene,
- scattered outliers across the viewing volume,
- low overall density.
  
### LichtFeld Studio — Raw Output

The LichtFeld Studio output contains:

- a very dense central reconstruction,
- extensive peripheral streaks,
- large spatial spread,
- numerous floating structures.

### Summary of Visual Findings (Before Cleaning)

Across all raw reconstructions:

- **Inria** and **OpenSplat** generated comparatively **more compact scene
  volumes**.
- **gsplat** and **LichtFeld Studio** exhibited the **largest spatial spread**
  and most pronounced peripheral artifacts.
- **Nerfstudio** produced the sparsest models, but with **isolated distant
  clusters** affecting global compactness.
- All pipelines benefit significantly from a dedicated cleaning stage prior to
  deployment in real-time or immersive applications.

</details>

---

<details>
<summary><strong>▶ Visual Inspection — After Cleaning (to be completed)</strong></summary>

<br>

This section will present qualitative comparisons between raw and cleaned
reconstructions, including side-by-side screenshots and orbit renders, as well
as observations on noise removal, spatial compactness, and suitability for
real-time or immersive visualization.

</details>
