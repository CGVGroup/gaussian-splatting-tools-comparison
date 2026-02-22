# XR Viewers Comparative Analysis

This document presents a comparative analysis of two XR Gaussian Splatting viewers, evaluated under identical hardware conditions using models trained from a single dataset across five different training pipelines.

---

## Experimental Protocol

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

### Dataset and Reconstructions

Five different reconstructions of the same indoor environment were used for this analysis.  
They were generated using the following open-source Gaussian Splatting trainers: Inria gaussian-splatting, gsplat, OpenSplat, Nerfstudio, LichtFeld Studio.

All reconstructions were cleaned prior to evaluation to ensure consistency across viewers.

Detailed information about the recontructions, the cleaning procedures applied and their impact on the exported `.ply` files  can be found in the following document: [trainers/indoor.md](https://github.com/ernesta-sichetti/gaussian-splatting-tools-comparison/blob/main/trainers/indoor.md)

### Rationale for Indoor Environment Selection

The decision to use an indoor environment for viewer analysis is based on the comparative observations reported in: [trainers/indoor-vs-outdoor.md](https://github.com/ernesta-sichetti/gaussian-splatting-tools-comparison/blob/main/trainers/indoor-vs-outdoor.md)

In summary:

- more stable reconstruction results across different trainers,
- more consistent visual outputs,
- fewer large-scale structural inconsistencies compared to the outdoor case.

For these reasons, the indoor reconstructions were chosen as the reference dataset for evaluating XR viewer behavior.

### Hardware and Viewing Setup

Evaluations were perfomerd under the following conditions:

- A system running **Windows 11** equipped with a **single NVIDIA RTX 4060 GPU**.
- Immersive testing was carried out using a **Meta Quest 3 headset**.

</details>

---

## Visual Inspection Protocol

A qualitative visual inspection was conducted to compare the rendering behavior of **Unity-VR-Gaussian-Splatting (ninjamode)** and **GaussianSplattingVRViewerUnity (clarte53)**

All assessments were performed using a 5-point ordinal scale (1–5), where:

- **1** indicates severe and clearly visible artifacts,
- **3** indicates moderate but tolerable instability,
- **5** indicates stable and visually coherent rendering.

The evaluation focused on the following criteria:

- **Surface Solidity** — geometric continuity and absence of holes or fragmentation.
- **Motion Parallax Stability** — depth consistency during head movement.
- **Edge Stability** — absence of flickering or shimmering contours.
- **Layering / Transparency Behavior** — correct depth ordering and occlusion handling.
- **Near-Object Robustness** — rendering stability during close inspection.

---

## Visual Inspection

## 1. Unity-VR-Gaussian-Splatting (ninjamode)

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

## Visual Inspection Evaluation

| Trainer | Surface Solidity | Motion Parallax Stability | Edge Stability | Layering / Transparency | Near-Object Robustness |
|----------|------------------|---------------------------|----------------|------------------------|------------------------|
| Inria gaussian-splatting | 5 | 5 | 4 | 4 | 4 |
| gsplat | 4 | 5 | 4 | 4 | 4 |
| OpenSplat | 4 | 5 | 4 | 3 | 5 |
| Nerfstudio | 2 | 4 | 3 | 2 | 3 |
| LichtFeld Studio | 3 | 2 | 2 | 2 | 3 |

## Observations

### Inria gaussian-splatting

Surfaces appear solid and structurally coherent across walls, furniture, and floors, with no visible holes or fragmentation. Black spike-like artifacts are observed on the table during head rotation.  
When inspecting objects at extremely close range, slight surface compression is observed. Overall, Inria provides the most stable reconstruction in Ninjamode, with only minor depth ordering artifacts during rotation.

### gsplat

Surfaces remain stable but appear slightly more fluid compared to Inria, and similar spike-like artifacts occasionally appear during movement. Close-range inspection reveals visible splats at extreme proximity.  
gsplat behaves similarly to Inria but with slightly reduced surface rigidity.

### OpenSplat

Surfaces appear moderately fluid, and colored splats located behind walls become visible during movement. Contours remain generally stable, and near-object inspection behaves slightly better than in Inria and gsplat.  
The main limitation of OpenSplat in Ninjamode is layering instability during motion.

### Nerfstudio

There are visible holes in walls and structural discontinuities, while the floor exhibits transparency artifacts. Splats are more visible on near objects than in the previously evaluated pipelines.  
Nerfstudio shows reduced geometric solidity and transparency consistency compared to the other trainers.

### LichtFeld Studio

Brown splats from foreground objects occasionally appear projected onto background walls. Diffuse vibration is visible across the entire scene during movement and rotation. The scene appears to slide or drag during head motion.  
Contours may vibrate, possibly related to global motion instability.  
LichtFeld Studio exhibits global motion instability and depth ordering issues not observed in the other trainers.
</details>

---

## 2. GaussianSplattingVRViewerUnity (clarte53)

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

## Visual Inspection Evaluation

| Trainer | Surface Solidity | Motion Parallax Stability | Edge Stability | Layering / Transparency | Near-Object Robustness |
|----------|------------------|---------------------------|----------------|------------------------|------------------------|
| Inria gaussian-splatting | 5 | 3 | 3 | 4 | 4 |
| gsplat | 4 | 3 | 3 | 3 | 4 |
| OpenSplat | 4 | 3 | 3 | 3 | 4 |
| Nerfstudio | 1 | 4 | 2 | 1 | 3 |
| LichtFeld Studio | 3 | 1 | 4 | 3 | 3 |

---

## Observations

### Inria gaussian-splatting

Surfaces appear solid and structurally coherent across walls, furniture, and floors. The scene exhibits slownes during head movement and rotation, producing a dragging effect and visual artifacts that seem related to view stability rather than splat noise.  
Contours appear slightly vibratory, and the entire scene gives the impression of vibrating during motion. A sensation of parallax instability is perceived during movement, and a very slight transparency is noticeable in some areas.  
Inria maintains strong geometric solidity but exhibits perceptible motion instability and minor transparency artifacts.

### gsplat

Surfaces are generally stable but appear slightly fluid during movement and head rotation. Walls exhibit slight but visible transparency. A sensation of parallax instability is perceived during movement, as observed in Inria.  
gsplat presents surface solidity but shows noticeable motion instability and transparency artifacts.

### OpenSplat

Surfaces appear moderately fluid, and transparency artifacts are visible during movement. The scene exhibits slowness during head motion, as observed in Inria and gsplat, affecting visual coherence. Contours remain generally stable.  
OpenSplat presents acceptable geometric solidity but shows recurring transparency artifacts and motion-related instability.

### Nerfstudio

Walls appear perforated, with visible holes and structural discontinuities. The floor exhibits transparency artifacts, and splats are clearly visible on near objects. The overall reconstruction appears geometrically degraded.  
Nerfstudio exhibits strong structural instability and reduced surface coherence, while providing better motion stability than the previously evaluated trainers.

### LichtFeld Studio

The scene runs extremely slowly, although the static visual quality appears high. Brown artifacts are present, and contours appear well defined. The scene drags noticeably during head motion.  
LichtFeld Studio provides high static detail but suffers from severe motion instability and usability limitations.

</details>

---
