# Indoor vs Outdoor — Comparative Analysis

This section compares the behavior of the evaluated Gaussian Splatting pipelines across two different scene categories: an indoor and an outdoor setting.
The comparison focuses on differences in:

- raw reconstruction structure,
- background modeling,
- artifact distribution,
- cleaning difficulty,
- quantitative impact of post-processing,
- emerging trends across environments.

All results discussed here are derived from the detailed analyses reported in `indoor.md` and `outdoor.md`.

---

## Scope of the Comparison

The same five pipelines—Inria GS, gsplat, OpenSplat, Nerfstudio, and LichtFeld Studio—were evaluated on one **indoor scene** and one **outdoor scene**.

For both scenarios:

- training was executed on the **same hardware platform**,
- reconstructions were **exported** to `.ply` format,
- a **quantitative benchmarking analysis** was conducted,
- **qualitative inspection** and **post-processing** were performed using **SuperSplat**,
- similar **cleaning principles** were applied, with parameters adapted to each scene.

The objective of this comparison is to highlight how each pipeline behaves when confronted with markedly different spatial layouts.

---

## Quantitative Benchmarking Comparison

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

| Tool | Gaussians Indoor | Gaussians Outdoor | Size Indoor (MB) | Size Outdoor (MB) | Training Indoor (min) | Training Outdoor (min) |
|------|----------------:|-----------------:|----------------:|-----------------:|---------------------:|----------------------:|
| Inria GS | 955,819 | 777,067 | 226.1 | 183.8 | 120 | 60 |
| gsplat | 1,265,239 | 1,031,707 | 284.8 | 232.2 | 50 | 45 |
| OpenSplat | 510,870 | 589,291 | 120.8 | 139.4 | 60 | 50 |
| Nerfstudio | 170,150 | 197,545 | 40.2 | 46.7 | 30 | 25 |
| LichtFeld Studio | 1,000,000 | 1,000,000 | 236.5 | 236.5 | 60 | 50 |

### Observations

- **Inria GS** produces fewer Gaussians and a smaller model for the outdoor scenario, together with a substantially shorter training time.

- **gsplat** remains the densest pipeline in both scenarios, with consistently large models and only limited differences between indoor and outdoor.

- **OpenSplat** increases both Gaussian count and file size for the outdoor scenario.

- **Nerfstudio** remains the most compact pipeline in both settings, although the outdoor scene leads to a slightly larger model.

- **LichtFeld Studio** reaches the same Gaussian budget in both scenarios, but trains faster on the outdoor one.

</details>

---

## Raw Reconstructions: Structural Differences

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

Clear structural differences emerge between indoor and outdoor raw reconstructions.

### Indoor

Indoor scenes are characterized by:

- compact spatial envelopes,
- well-defined scene boundaries,
- limited far-field geometry,
- peripheral Gaussians not associated with interior geometry,
- streak artifacts mostly concentrated near the main structure.

Across pipelines, raw indoor reconstructions tend to occupy **relatively constrained volumes**, making spurious splats clusters easier to identify.

### Outdoor

Outdoor scenes present fundamentally different challenges:

- large spatial extents,
- far-field environmental geometry corresponding to sky and vegetation,
- peripheral halos of Gaussians around the main structure,
- elongated streak artifacts extending from the core,
- detached background elements that may still represent valid scene content.

Compared to indoor cases, outdoor raw reconstructions consistently exhibit **wider spatial footprints** and **more complex splats distributions**.

</details>

---

## Cleaning Difficulty and Strategies

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

The qualitative differences between indoor and outdoor scenes are reflected directly in the **complexity of post-processing**.

### Indoor Cleaning

Indoor cleaning operations were **generally straightforward**:

- the scene core was easy to localize,
- bounding volumes could be tightly defined,
- aggressive spatial restriction was often feasible,
- many peripheral Gaussians could be removed without damaging structural elements,
- manual refinement was limited and primarily used for final inspection.

As a result, large portions of removable geometry could be pruned confidently.

### Outdoor Cleaning

Outdoor cleaning proved substantially **more challenging**:

- far-field Gaussians frequently corresponded to valid environmental elements,
- background geometry often formed detached clusters rather than continuous shells,
- sky-related streaks and large surface Gaussians required careful handling,
- aggressive spatial pruning risked removing legitimate scene content,
- manual inspection and selective filtering were critical at later stages.

Cleaning strategies therefore remained conservative, prioritizing preservation of environmental context over maximal size reduction.

</details>

---

## Quantitative Impact of Cleaning

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

| Tool | Δ Gaussians Indoor (%) | Δ Size Indoor (%) | Δ Gaussians Outdoor (%) | Δ Size Outdoor (%) |
|------|-----------------------:|------------------:|------------------------:|-------------------:|
| Inria GS | −45.8 | −45.8 | −52.5 | −52.4 |
| gsplat | −45.4 | −45.4 | −46.1 | −46.1 |
| OpenSplat | −21.2 | −21.2 | −21.3 | −21.3 |
| Nerfstudio | −25.4 | −25.4 | −13.5 | −13.5 |
| LichtFeld Studio | −20.0 | −20.0 | −58.4 | −58.3 |

## Observations

The numerical effect of post-processing differs markedly between indoor and outdoor settings.

In **indoor reconstructions**:

- **Inria GS** and **gsplat** experience the largest reductions in Gaussian count and file size (≈ −45%),
- **OpenSplat** and **LichtFeld Studio** show more moderate decreases (≈ −21% and ≈ −20%),
- **Nerfstudio** occupies an intermediate regime (≈ −25%).

In **outdoor reconstructions**:

- reductions are more variable across pipelines, ranging from limited (**Nerfstudio**, ≈ −13%) to very large (**LichtFeld Studio**, ≈ −58%),
- **Inria GS** exhibits a substantial reduction after cleaning (≈ −52%),
- **gsplat** also undergoes a strong reduction (≈ −46%),
- **OpenSplat** again shows a moderate reduction (≈ −21%).

These results confirm that post-processing efficiency depends strongly on the spatial organization of the raw reconstruction.

Across environments, **LichtFeld Studio** and **Inria GS** are pruned more aggressively in outdoor reconstructions than in the indoor reconstructions, while **gsplat** and **OpenSplat** show smaller cross-scenario differences.

**Nerfstudio** behaves differently across settings: it undergoes moderate pruning in the indoor scene but only limited reductions in the outdoor scene, suggesting that a larger fraction of its outdoor Gaussians contribute to retained background geometry and cannot be safely removed without degrading scene completeness.


</details>

---

## Qualitative Impact of Cleaning

<details open>
<summary><strong>Show / Hide Section</strong></summary>

<br>

Visual inspection across the two scenarios reveals systematic differences in how the same pipelines distribute Gaussians and how effectively artifacts can be controlled.

### Raw Reconstructions

Across tools, the indoor scenes generally produced more spatially constrained raw outputs than outdoor scenes:

- **Inria GS** remains relatively compact in both environments, but the outdoor reconstruction contains more detached peripheral elements and streak artifacts than indoors.

- **gsplat** remains relatively compact in both environments, but the outdoor scene includes detached environmental splats and more far-field streaks extending from the scene core.

- **OpenSplat** produces streak artifacts and peripheral clusters in both scenarios; however, in the outdoor scene these form broader halos compared to the indoor case.

- **Nerfstudio** is characterized by sparse central reconstructions in both environments, with a wide halo of streaks, spike-like structures, and clusters of splats.

- **LichtFeld Studio** reconstructs dense scene cores in both settings, with a large spatial footprint and peripheral halo (stronger in the outdoor reconstructuion).

Overall, outdoor raw reconstructions systematically contain larger far-field regions, halos around the scene core, and the sky reconstruction is a dominant source of large Gaussians and streak artifacts.

---

### After Cleaning

Post-processing reduces clutter in both scenarios, but different residual patterns remain:

- **Inria GS** converges to a compact reconstruction in both environments, though the outdoor scene retains more peripheral streaks than the indoor scene.

- **gsplat** becomes tightly focused after cleaning in the indoor scene, whereas the outdoor reconstruction still preserves some streak artifacts near the core and in sky-related regions.

- **OpenSplat** achieves strong spatial compaction in both cases, but the outdoor scene continues to show more significant residual elongated structures than the indoor one.

- **Nerfstudio** transitions to very tightly cropped reconstructions in both environments, with the outdoor scene still retaining peripheral remnants and streaks.

- **LichtFeld Studio** shows the largest qualitative contrast: while the indoor scene becomes compact with only faint residual halos, the outdoor scene preserves a dense core surrounded by thin but persistent peripheral halos.

In summary, **indoor reconstructions** tend to converge toward tightly bounded reconstructions after cleaning across pipelines, whereas **outdoor reconstructions** consistently preserve larger envelopes and detached structures, reflecting the increased complexity of environmental geometry and sky regions.

</details>


---

## Summary

- **Outdoor reconstructions** show **larger far-field regions** and **more complex splats distributions** than indoor reconstructions.
- **Indoor cleaning** is generally **more straightforward** due to clearer scene boundaries and a more localized scene core.
- **Outdoor cleaning** requires **more cautious and selective pruning**, since far-field Gaussians may correspond to **valid environmental elements** (e.g., sky and vegetation).
- Quantitatively, **outdoor post-processing amplifies inter-method variability** (e.g., very large reductions for **LichtFeld Studio** and **Inria GS**, but limited reductions for **Nerfstudio**), while some pipelines (e.g., **OpenSplat**) show similar reduction rates across environments.
- Combining indoor and outdoor benchmarks highlights tool behaviors that would not be visible when evaluating a single scene category.


