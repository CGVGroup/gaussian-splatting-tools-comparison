# Which Trainer Should I Use?

A quick-decision guide distilled from the [trainer benchmarks](../analysis/trainers/methodology.md).
If you want the full evidence, read [Indoor](../analysis/trainers/indoor.md),
[Outdoor](../analysis/trainers/outdoor.md), and
[Indoor vs Outdoor](../analysis/trainers/indoor-vs-outdoor.md). This page is the short answer.

!!! note "What 'quality' means here"
    These benchmarks did **not** compute perceptual fidelity metrics (PSNR / SSIM / LPIPS).
    All five pipelines were trained to 30,000 iterations with default hyperparameters, and were
    compared on **structural** and **qualitative** grounds: Gaussian count, exported `.ply` size,
    training time, raw-artifact distribution, and how cleanly each model could be post-processed
    in SuperSplat. "Quality" below refers to structural compactness and cleanliness, not measured
    image fidelity. All numbers come from a single **RTX 4060 / Windows 11** machine, one indoor
    and one outdoor 151-frame DL3DV scene.

---

## TL;DR

| If you want… | Use | Why |
|---|---|---|
| **The fastest, lightest result** | **Nerfstudio** | Shortest training time (25–30 min) and by far the smallest output (40–47 MB); stays compact with little cleaning. |
| **The best speed-to-density balance** | **gsplat** | Densest reconstruction of all five, yet the fastest of the high-density trainers (45–50 min). |
| **A predictable, fixed-budget model** | **LichtFeld Studio** | MCMC densification caps the model at exactly 1 M Gaussians in both scenes; fast and consistent. |
| **The least post-processing effort** | **OpenSplat** | Most stable across scenes — raw output already sits mostly inside the kept envelope (~21% removed by cleaning). |
| **The canonical reference** | **Inria gaussian-splatting** | The original 3DGS implementation; structurally stable but the slowest. |

---

## Numbers at a glance

Results per scene (RTX 4060, 30k iterations, default hyperparameters):

| Trainer | Gaussians (in / out) | Size MB (in / out) | Train min (in / out) | Cleaning Δ (in / out) |
|---|---|---|---|---|
| **Nerfstudio** | 170k / 198k | 40 / 47 | **30 / 25** | −25% / −13% |
| **gsplat** | **1.27M / 1.03M** | **285 / 232** | 50 / 45 | −45% / −46% |
| **OpenSplat** | 511k / 589k | 121 / 139 | 60 / 50 | −21% / −21% |
| **LichtFeld Studio** | 1.00M / 1.00M | 237 / 237 | 60 / 50 | −20% / −58% |
| **Inria** | 956k / 777k | 226 / 184 | **120 / 60** | −46% / −52% |

*in = indoor, out = outdoor. Cleaning Δ = percentage of Gaussians removed by SuperSplat post-processing.*

---

## Trade-offs in detail

### Training speed

- **Nerfstudio** is the fastest in absolute terms (25–30 min).
- **gsplat** is the fastest trainer that still produces a dense model (45–50 min).
- **Inria** is the slowest — 120 min indoors, roughly double the others on this hardware. Treat it as a reference baseline rather than a production choice.

### Output size and density

- **gsplat** produces the densest, heaviest models; **LichtFeld Studio** is close but fixed at a 1 M Gaussian budget.
- **Nerfstudio** is an order of magnitude lighter than the rest — useful when storage, download bandwidth, or runtime (e.g. XR rendering) is a constraint.
- Storage cost per 100k Gaussians is nearly identical across tools (~22–24 MB), so file size tracks Gaussian count rather than encoding efficiency.

### Cleanliness of the raw model

- **OpenSplat** loses only ~21% to cleaning in *both* scenes — its raw output is already well-contained.
- **Inria** and **gsplat** carry ~45–46% removable peripheral splats indoors; much of their raw size is clutter.
- **LichtFeld Studio** is the most scene-dependent: only ~20% removable indoors but ~58% outdoors.

### Hardware

All results are from a single **RTX 4060** — a mid-range consumer GPU — so none of these trainers requires datacenter hardware for 151-frame scenes. All five are command-line / framework-level tools targeting Windows or Linux. If you want a one-click GUI experience, see the commercial trainers covered in [Overview / Trainers](trainers.md).

---

## Indoor vs outdoor

- **Indoor** scenes are forgiving: tight boundaries make cleaning straightforward and aggressive pruning safe. Most tools converge to compact models regardless of starting density. Optimize for **speed and size** — **Nerfstudio** or **gsplat** are strong picks.
- **Outdoor** scenes are harder: far-field sky and vegetation are legitimate content, so cleaning must stay conservative. **OpenSplat** and **Nerfstudio** keep a large share of their Gaussians as valid background (low cleaning Δ), while **LichtFeld Studio** and **Inria** carry a lot of removable peripheral clutter. For unconstrained scenes, favor a trainer whose raw output is already well-bounded and expect more manual cleanup.

!!! tip "Note on the XR pipeline"
    For the downstream XR-viewer evaluation, the **indoor** scene was used as the reference,
    because its bounded geometry gives a more controlled and consistent benchmark across pipelines.

---

## Bottom line

- **Default pick for most users:** **gsplat** — fast, dense, well-supported, and the CUDA backend behind Nerfstudio's Splatfacto model.
- **When size or runtime is king (e.g. XR streaming):** **Nerfstudio**.
- **When you want a fixed, predictable output budget:** **LichtFeld Studio**.
- **When you want the least post-processing effort, especially outdoors:** **OpenSplat**.
- **When you need the canonical 3DGS reference:** **Inria gaussian-splatting** — accept the longer training time.

Setup instructions for each trainer are in the [How-To guides](../how-to/index.md).
