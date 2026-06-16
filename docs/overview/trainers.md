# Gaussian Splatting Trainers Overview

This document provides a qualitative overview of Gaussian Splatting training pipelines, including both commercial and open-source tools.  

The term **viewer** refers to the ability to interactively navigate the reconstructed scene after training, rather than to a scientific inspection tool.

Qualitative labels such as *Low*, *Medium*, and *High* do not represent absolute performance metrics.  
They are comparative assessments derived from practical experimentation and documentation review, and should be interpreted as relative indicators within this comparison rather than objective measurements.

---

## Overview Tables

### Commercial Trainers

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer + Viewer | **Luma AI** | trainer, online, commercial, web-view | No | Cloud | Video / Images | Proprietary, `.ply` (mesh / point cloud export) | Low | Medium | Easy | Fully automated cloud-based pipeline |
| Trainer + Viewer | **Teleport** | trainer, online, commercial, immersive-view | No | Cloud | Video / Images | Proprietary | Variable | Medium | Easy | Desktop and VR-oriented cloud-based pipeline |
| Trainer + Viewer | **Polycam** | trainer, online, mobile, commercial, web-view | No | iOS / Android / Cloud | Video / Images | Multiple 3D formats (.ply only in paid plans) | Medium | Low | Easy | Mobile and cloud-enabled GS pipeline |
| Trainer + Viewer | **Postshot** | trainer, desktop, commercial, desktop-view | No | Windows | Images / Video | `.ply`, `.spz`, `.psht` | High | Medium | Medium |Local desktop GS training; .ply export available only in paid plans |
| Trainer + Viewer | **Scaniverse** | trainer, mobile, commercial, immersive-view | No | iOS / Android | Video | `.ply`, `.spz` | Medium | Low | Easy | On-device GS training with direct .ply export |

### Open-Source Trainers

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer | **Inria gaussian-splatting** | trainer, desktop, open-source | Yes | Windows / Linux | Images | `.ply` | High | High | Advanced | Reference open-source implementation |
| Trainer | **gsplat** | trainer, backend, desktop, open-source, library | Yes | Windows / Linux | Images | `.ply` | High | Medium | Advanced | High-performance PyTorch library for GS |
| Trainer + Viewer | **Nerfstudio** | trainer, desktop, open-source, framework | Yes | Windows / Linux | Images / Video / COLMAP | `.ply` | Low | Low | Advanced | Modular framework |
| Trainer | **OpenSplat** | trainer, desktop, open-source | Yes | Windows / Linux | Images (COLMAP / Nerfstudio) | `.ply` | Medium | Medium | Advanced | C++ native GS implementation based on LibTorch |
| Trainer + Viewer | **LichtFeld Studio** | trainer, desktop, open-source | Yes | Windows / Linux | Images / Video / COLMAP | `.ply`, `.spz`, `.lfs` | High | Medium | Advanced | Optimized C++ trainer |

---

## Tool Notes

### Commercial Trainers

**[Luma AI](https://lumalabs.ai/interactive-scenes)** — Fully automated cloud-based pipeline. Scenes are stored in a proprietary format optimised for real-time rendering; export to `.ply` is available and scenes can be explored interactively via the web interface.

**[Teleport](https://teleport.varjo.com/)** ([docs](https://teleport.varjo.com/docs/)) — Cloud reconstruction platform by Varjo designed for spatial visualization. Allows interactive navigation of reconstructed environments in browser; `.ply` export available only in paid plans.

**[Polycam](https://poly.cam/tools/gaussian-splatting)** ([guide](https://learn.poly.cam/hc/en-us/articles/30549121659412-How-to-Create-Photogrammetry-Models-From-Images-and-Videos-on-Polycam-Web)) — Mobile and web capture platform supporting photogrammetry and Gaussian Splatting workflows. Models can be previewed online; point-cloud export formats (including `.ply`) depend on subscription tier.

**[Postshot](https://www.jawset.com/)** ([docs](https://www.jawset.com/docs/d/Postshot+User+Guide) · [video](https://www.youtube.com/watch?v=ERuRMOVO58Q)) — Local desktop GS training with configurable reconstruction parameters and integrated scene viewing after training. `.ply` export available only in paid plans.

**[Scaniverse](https://scaniverse.com/)** — On-device GS training performed locally on mobile. Scenes can be interactively explored on mobile and in VR applications; direct `.ply` and `.spz` export supported.

### Open-Source Trainers

**[Inria gaussian-splatting](https://github.com/graphdeco-inria/gaussian-splatting)** — Reference research implementation of the original 3D Gaussian Splatting method.

**[gsplat](https://github.com/nerfstudio-project/gsplat)** — CUDA/PyTorch library implementing Gaussian Splatting operations. Serves as backend for higher-level frameworks such as Nerfstudio.

**[Nerfstudio](https://github.com/nerfstudio-project/nerfstudio)** ([docs](https://docs.nerf.studio/index.html)) — Modular training framework for neural fields with an integrated web viewer for real-time monitoring and scene navigation. Uses `gsplat` as CUDA backend for its Splatfacto model.

**[OpenSplat](https://github.com/pierotofy/OpenSplat)** — Standalone C++ GS implementation using LibTorch, removing Python dependencies for portability. Supports multiple dataset formats including COLMAP, Nerfstudio, and OpenMVG.

**[LichtFeld Studio](https://github.com/MrNeRF/LichtFeld-Studio)** ([website](https://lichtfeld.io/)) — Standalone C++ training suite with integrated real-time visualization and a GUI for monitoring training progress and navigating the reconstructed scene.

---
