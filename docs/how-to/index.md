# How-To Guides

Step-by-step setup and execution instructions for each tool included in the experimental analysis. Each guide covers prerequisites, installation, dataset preparation, training/loading, and export.

---

## Trainer guides

| Tool | Platform | Prerequisites | Difficulty |
|------|----------|---------------|------------|
| [Inria gaussian-splatting](trainers/inria.md) | Windows / Linux | CUDA, Python, Conda, Git | Medium |
| [gsplat](trainers/gsplat.md) | Windows / Linux | CUDA, Python, Conda | Medium |
| [OpenSplat](trainers/opensplat.md) | Windows / Linux | CUDA, CMake, LibTorch, OpenCV | Advanced |
| [Nerfstudio](trainers/nerfstudio.md) | Windows / Linux | CUDA, Python, Conda | Medium |
| [LichtFeld Studio](trainers/lichtfeldstudio.md) | Windows | NVIDIA GPU ≥ 8 GB VRAM | Easy (precompiled binary) |

All trainer guides assume a pre-computed COLMAP reconstruction or provide instructions for running COLMAP as part of the pipeline. The guides are written for and tested on Windows 11 with an NVIDIA RTX 4060.

---

## Viewer guides

| Tool | Platform | Prerequisites | Difficulty |
|------|----------|---------------|------------|
| [Unity-VR-Gaussian-Splatting (ninjamode)](viewers/ninjamode.md) | Windows VR | Unity 2022.3.47f1, VR headset | Advanced |
| [GaussianSplattingVRViewerUnity (clarte53)](viewers/clarte53.md) | Windows VR | VR-ready PC, RTX 2060+, VR headset | Medium (precompiled binary available) |

Both viewer guides assume you already have a cleaned `.ply` file produced by one of the trainer guides above.

---

## Contributing a new guide

To add a guide for a new tool:

1. Create a new `.md` file in `docs/how-to/trainers/` or `docs/how-to/viewers/` following the structure of an existing guide (prerequisites → installation → dataset preparation → execution → export)
2. Add a row to the table above in this file
3. Add an entry to the `nav:` section of `mkdocs.yml`

See [CONTRIBUTING.md](../../CONTRIBUTING.md) at the repository root for full guidelines.
