# Outdoor Dataset — Benchmark Results and Visual Inspection

This section reports quantitative benchmarking results for several open-source Gaussian Splatting implementations evaluated on the same outdoor dataset: Inria, gsplat, OpenSplat, nerfstudio and Lichtfeld Studio.

---

## Dataset Description

The outdoor dataset consists of **151 frames** extracted from the following video sequence:

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

## Quantitative Results

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

| Tool | Output Size (MB) | # Gaussians | MB / 100k | Training Time (min) | Min / 100k | Densification Strategy | Discussion |
|------|----------------:|------------:|----------:|-------------------:|-----------:|----------------------|------------|
| Inria GS | 188 | 777,067 | 24.2 | 60 | 7.7 | Adaptive density control | [How-To](../tools/inria.md) |
| gsplat | 238 | 1,031,707 | 23.0 | 45 | 4.4 | CUDA-optimized default | [How-To](../tools/gsplat.md) |
| OpenSplat | 143 | 589,291 | 24.3 | 50 | 8.5 | Native pruning | [How-To](../tools/opensplat.md) |
| Nerfstudio | 48 | 19,754 | 243.1 | 25 | 126.6 | Adaptive culling + gsplat backend | [How-To](../tools/nerfstudio.md) |
| LichtFeld Studio | 242 | 1,000,000 | 24.2 | 50 | 5.0 | MCMC pipeline | [How-To](../tools/lichtfeld.md) |

---

## Observations

- **Nerfstudio** produced the most compact representation, with the lowest Gaussian count and smallest output size, and shortest training time, at the cost of reduced Gaussian density.

- **OpenSplat** achieved a favorable compromise between reconstruction density and output size through aggressive pruning.

- **gsplat** and **LichtFeld Studio** generated the densest reconstructions, with correspondingly larger output files.

- The original **Inria GS** reference implementation remained slower than the other pipelines, although it produced stable large-scale outdoor reconstructions.

</details>

---

## Qualitative Evaluation Protocol

Beyond quantitative benchmarking, a qualitative evaluation was conducted on all reconstructed scenes.

Each raw `.ply` output was first inspected visually using **SuperSplat** in order to assess noise distribution and structural coherence.

Subsequently, scene-cleaning was applied to all models. The cleaned reconstructions were then re-inspected using **SuperSplat** to enable direct visual comparison between raw and post-processed outputs.

This two-stage inspection protocol supports the qualitative analyses and visual materials presented in the following sections.

## Visual Inspection — Before Cleaning (Raw Reconstructions)

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

Before applying any post-processing, all reconstructed Gaussian Splatting models were visually inspected in the **SuperSplat Editor** using their exported `.ply` files.

The goal of this inspection was:

- to evaluate the **spatial compactness** of the reconstruction,
- to analyze the **distribution of outlier Gaussians**,
- to identify large-scale **floating artifacts**,
- and to qualitatively assess differences between the analyzed tools prior to any cleaning operations.

All figures in this section correspond to screenshots captured in SuperSplat.

### Inria Gaussian Splatting — Raw Output

The raw reconstruction produced by Inria shows a clearly identifiable central outdoor structure. When visualized to encompass the full extent of the model, Gaussians populate far-field background regions corresponding to distant environmental elements such as vegetation. The distribution remains relatively coherent, with streak-like artifacts visible near the central structure and within distant background regions. In addition, some large-scale Gaussians are present in upper regions of the reconstruction, corresponding to portions of the sky.

![Inria raw view 1](../media/outdoor/inria/raw/inria_00.png)
![Inria raw view 2](../media/outdoor/inria/raw/inria_01.png)

---

### gsplat — Raw Output

The gsplat raw reconstruction shows a clearly identifiable central outdoor structure. When visualized to encompass the full extent of the model, far-field background regions corresponding to environmental elements appear less spatially compact than in Inria, and a large number of long vertical streaks and floating clusters are visible near the scene core. In addition, both large-scale Gaussians and thin streaks are present in upper regions of the reconstruction, corresponding to portions of the sky. 

![gsplat raw view 1](../media/outdoor/gsplat/raw/gsplat_00.png)
![gsplat raw view 2](../media/outdoor/gsplat/raw/gsplat_01.png)

---

### OpenSplat — Raw Output

The OpenSplat raw reconstruction shows a clearly identifiable central outdoor structure. When visualized to include the full extent of the model, the scene envelope appears moderately compact, but numerous elongated streak-like artifacts extend outward from the main structure. Far-field background regions are present but remain less spatially dispersed than in gsplat and Nerfstudio, while isolated floating clusters are comparatively limited. Prominent streaks are visible in upper regions of the reconstruction, corresponding to portions of the sky

![OpenSplat raw view 1](../media/outdoor/opensplat/raw/opensplat_00.png)
![OpenSplat raw view 2](../media/outdoor/opensplat/raw/opensplat_01.png)

---

### Nerfstudio — Raw Output

The Nerfstudio raw reconstruction shows a clearly identifiable central outdoor structure. When visualized to encompass the full extent of the modeln,  the scene envelope appears moderately compact, but numerous elongated streaks extend outward from the scene core, together with several isolated floating clusters distributed well beyond the main reconstructed region. In addition, upper portions of the reconstruction contain large-scale Gaussians and spike-like artifacts corresponding to sky regions. Overall, the reconstruction appears globally extended.

![Nerfstudio raw view 1](../media/outdoor/nerfstudio/raw/nerfstudio_00.png)
![Nerfstudio raw view 2](../media/outdoor/nerfstudio/raw/nerfstudio_01.png)

---
  
### LichtFeld Studio — Raw Output

The raw reconstruction produced by LichtFeld Studio shows a dense outdoor scene. When visualized at full scene extent, both structural Gaussians and removable background components remain spatially concentrated around the scene core rather than dispersing into distant clusters. Streaks corresponding to portions of the sky are visible in the upper region, together with larger surface Gaussians located further above. Overall, the raw output appears highly concentrated in space but globally extended, with most geometry (both relevant and removable) remaining locally grouped.

![Lichtfeld raw view 1](../media/outdoor/lichtfeldstudio/raw/lichtfeldstudio_00.png)
![Lichtfeld raw view 2](../media/outdoor/lichtfeldstudio/raw/lichtfeldstudio_01.png)
![Lichtfeld raw view 3](../media/outdoor/lichtfeldstudio/raw/lichtfeldstudio_02.png)

---

### Summary of Visual Findings (Before Cleaning)

Across all raw reconstructions:

- All pipelines reconstruct a clearly identifiable central outdoor structure and include far-field environmental Gaussians corresponding to vegetation or sky regions.
- Across all methods, elongated streaks and large surface Gaussians in upper regions representing the sky are consistently observed.
- **Inria** and **gsplat** exhibit comparatively limited artifact extent relative to the other pipelines, with gsplat slightly more spatially dispersed.
- **OpenSplat** presents numerous elongated streaks around the main structure, particularly in upper sky-related regions, while floating clusters are comparatively limited.
- **Nerfstudio** produces the lightest model but shows many elongated streaks and several isolated floating clusters far from the central structure, resulting in a globally extended reconstruction.
- **LichtFeld Studio** reconstructs an extremely large scene volume while keeping most Gaussians—both structural and removable—spatially concentrated around the scene core.


</details>

---

## Scene Cleaning Procedure

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

After inspecting the raw reconstructions, all outdoor scenes were cleaned using **SuperSplat** with the goal of reducing removable Gaussians while preserving both the main scene structure and relevant environmental context.

The cleaning process was applied consistently across all five pipelines but proved inherently challenging due to the characteristics of the outdoor reconstructions. In all cases, environmental background elements (such as vegetation, sky regions, and ground reflections) were represented by Gaussians located at significant distances from the central scene core, often forming clusters rather than compact background layers. As a result, a straightforward spatial restriction of the scene volume was not always feasible without risking the removal of valid scene content.

In reconstructions with very large spatial extents, such as **LichtFeld Studio**, and with well defined clusters of spurious Gaussians, such as **Nerfstudio**,  an initial coarse spatial restriction was possible to reduce the recontruction volume or eliminate extreme outliers. However, subsequent refinement stages required careful, localized filtering, as many Gaussians located far from the central structure still corresponded to legitimate background geometry.

Furthermore, several Gaussians that visually appeared removable (such as elongated streaks, thin spike-like structures, or large-scale primitives) were found to contribute to meaningful elements of the scene, including portions of the sky, background vegetation, or ground reflections. This ambiguity made aggressive pruning unsuitable and necessitated a conservative and selective cleaning strategy.

The following operations were therefore applied:

- **Spatial restriction of the scene volume**, performed cautiously to reduce the reconstruction volume or to remove only clearly detached background clusters while preserving distant but valid environmental elements.
- **Distance-based pruning**, applied conservatively to eliminate Gaussians located well beyond the meaningful reconstruction envelope.
- **Opacity-based filtering**, removing low-opacity Gaussians with negligible visual contribution.
- **Scale-based filtering** on the Gaussian axes (scale *x*, *y*, *z*), used to identify and selectively remove abnormally large primitives not associated with stable scene geometry.
- **Surface-area filtering**, targeting oversized Gaussians that spanned large regions of space and did not correspond to coherent environmental elements.
- **Manual inspection and refinement**, required to disambiguate between removable artifacts and Gaussians representing valid background elements.
- **Export of the cleaned models** as new `.ply`.

</details>

---

## Scene Cleaning Evaluation

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

This table quantifies the impact of SuperSplat-based cleaning by comparing each raw reconstruction against its cleaned counterpart.

| Tool | Raw Gaussians | Cleaned Gaussians | Δ Gaussians (%) | Raw Size (MB) | Cleaned Size (MB) | Δ Size (%) |
|------|-------------:|------------------:|----------------:|--------------:|------------------:|-----------:|
| Inria GS | 867,000 | 866,617 | −0.04% | 231 | 209.9 | −9.1% |
| gsplat | 1,000,000 | 875,884 | −12.4% | 230 | 197.1 | −14.3% |
| OpenSplat | 510,000 | 273,368 | −46.4% | 123 | 66.2 | −46.2% |
| Nerfstudio | 170,000 | 126,841 | −25.4% | 43 | 30.7 | −28.6% |
| LichtFeld Studio | 1,000,000 | 800,515 | −20.0% | 242 | 189.3 | −21.8% |

## Observations

- **OpenSplat** shows the largest reduction after cleaning (≈ −46 % in both Gaussian count and file size).

- **Nerfstudio** exhibits a consistent decrease in both metrics while maintaining a compact representation, suggesting that its training pipeline already performs partial pruning but still benefits from post-processing.

- **gsplat** and **LichtFeld Studio** undergo moderate reductions after cleaning, with gsplat showing a smaller decrease than LichtFeld Studio, reflecting aggressive densification during training and the presence of removable background Gaussians in the raw outputs.

- **Inria GS** remains nearly unchanged after cleaning, which indicates that its reference implementation already produces structurally conservative and stable reconstructions with limited far-field noise.

</details>

---

## Visual Inspection — After Cleaning

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

This section focuses exclusively on the **post-cleaning appearance** of each model, highlighting changes in spatial compactness, peripheral noise removal, and preservation of structural detail.

This section presents both screenshots and screen-recorded orbit videos captured in SuperSplat after the cleaning procedure

### Inria Gaussian Splatting — Cleaned Output

The cleaned Inria reconstruction displays a highly compact central scene volume tightly aligned with the indoor region of interest. The overall spatial extent is reduced, with most peripheral outliers and far-field artifacts removed.

![Inria cleaned view 1](../media/indoor/inria/cleaned/inria_cleaned_00.png)
![Inria cleaned view 2](../media/indoor/inria/cleaned/inria_cleaned_01.png)

https://github.com/user-attachments/assets/978274ee-fd2b-4a49-ab45-47e485ae0420

---

### gsplat — Cleaned Output

The cleaned gsplat reconstruction exhibits a strongly compacted central scene volume and a markedly reduced overall spatial extent compared to the raw output. Most peripheral outliers and far-field artifacts have been removed, while dense interior regions and fine structural detail are preserved.

![gsplat cleaned view 1](../media/indoor/gsplat/cleaned/gsplat_cleaned_00.png)
![gsplat cleaned view 2](../media/indoor/gsplat/cleaned/gsplat_cleaned_01.png)

https://github.com/user-attachments/assets/f2dfa455-ae07-4eae-80f2-072601882549

---

### OpenSplat — Cleaned Output

The cleaned OpenSplat reconstruction presents a sharply delimited central scene volume with Gaussians concentrated almost exclusively inside the true interior region. The overall spatial extent is substantially reduced, with only minor peripheral outliers remaining near the scene boundaries.

![OpenSplat cleaned view 1](../media/indoor/opensplat/cleaned/opensplat_cleaned_00.png)
![OpenSplat cleaned view 2](../media/indoor/opensplat/cleaned/opensplat_cleaned_01.png)

https://github.com/user-attachments/assets/3b4954fa-b950-46ec-b32b-728b1f27b378

---

### Nerfstudio — Cleaned Output

The cleaned Nerfstudio reconstruction shows an extremely compact central scene volume and a very limited overall spatial extent. Peripheral outliers and far-field clusters are almost entirely eliminated, yielding a tightly cropped reconstruction while preserving the main architectural elements of the scene.

![Nerfstudio cleaned view 1](../media/indoor/nerfstudio/cleaned/nerfstudio_cleaned_00.png)
![Nerfstudio cleaned view 2](../media/indoor/nerfstudio/cleaned/nerfstudio_cleaned_01.png)

https://github.com/user-attachments/assets/880a85ce-ecfc-4653-ad6b-e24bb658ed49

---

### LichtFeld Studio — Cleaned Output

The cleaned LichtFeld Studio reconstruction now displays a compact central scene volume with a substantially reduced overall spatial extent. Most peripheral outliers and far-field artifacts have been removed, although faint residual halos remain visible near the scene boundaries, reflecting a conservative final pruning stage while maintaining dense interior structure.

![LichtFeld Studio cleaned view 1](../media/indoor/lichtfeldstudio/cleaned/lichtfeldstudio_cleaned_00.png)
![LichtFeld Studio cleaned view 2](../media/indoor/lichtfeldstudio/cleaned/lichtfeldstudio_cleaned_01.png)

https://github.com/user-attachments/assets/0363dd17-6705-4a65-b7c4-8bd875d3a325

---

## Summary of Visual Findings (After Cleaning)

After cleaning, the five pipelines exhibit different balances between noise removal, spatial compactness, and reconstruction density:

- **Inria GS** and **OpenSplat**, which already produced relatively compact raw reconstructions, further reduce their overall spatial extent after cleaning, leaving only minor peripheral remnants near the scene boundaries.

- **gsplat** and **LichtFeld Studio**, previously characterized by large spatial spread and extensive far-field clutter, now exhibit substantially tighter scene volumes, although dense interior regions and thin residual halos persist near the boundaries.

- **Nerfstudio**, which originally displayed sparse reconstructions with isolated distant clusters, presents a tightly cropped scene after cleaning while preserving the main architectural and furniture structures.

</details>

---

