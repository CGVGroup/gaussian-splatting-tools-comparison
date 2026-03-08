# Gaussian Splatting Trainers Overview

This document provides a qualitative overview of Gaussian Splatting training pipelines, including both commercial and open-source tools.  

The term **viewer** refers to the ability to interactively navigate the reconstructed scene after training, rather than to a scientific inspection tool.

Qualitative labels such as *Low*, *Medium*, and *High* do not represent absolute performance metrics.  
They are comparative assessments derived from practical experimentation and documentation review, and should be interpreted as relative indicators within this comparison rather than objective measurements.

---

## Overview Tables

### Commercial Trainers

<details open>
<summary><strong>Show / Hide Commercial Trainers</strong></summary>

<br>

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer + Viewer | **Luma AI** | trainer, online, commercial, web-view | No | Cloud | Video / Images | Proprietary, `.ply` (mesh / point cloud export) | Low | Medium | Easy | Fully automated cloud-based pipeline |
| Trainer + Viewer | **Teleport** | trainer, online, commercial, immersive-view | No | Cloud | Video / Images | Proprietary | Variable | Medium | Easy | Desktop and VR-oriented cloud-based pipeline |
| Trainer + Viewer | **Polycam** | trainer, online, mobile, commercial, web-view | No | iOS / Android / Cloud | Video / Images | Multiple 3D formats (.ply only in paid plans) | Medium | Low | Easy | Mobile and cloud-enabled GS pipeline |
| Trainer + Viewer | **Postshot** | trainer, desktop, commercial, desktop-view | No | Windows | Images / Video | `.ply`, `.spz`, `.psht` | High | Medium | Medium |Local desktop GS training; .ply export available only in paid plans |
| Trainer + Viewer | **Scaniverse** | trainer, mobile, commercial, immersive-view | No | iOS / Android | Video | `.ply`, `.spz` | Medium | Low | Easy | On-device GS training with direct .ply export |

</details>

### Open-Source Trainers

<details open>
<summary><strong>Show / Hide Open-Source Trainers</strong></summary>

<br>

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer | **Inria gaussian-splatting** | trainer, desktop, open-source | Yes | Windows / Linux | Images | `.ply` | High | Advanced | Advanced | Reference open-source implementation |
| Trainer | **gsplat** | trainer backend, desktop, open-source, library | Yes | Windows / Linux | Images | `.ply` | High | Medium | Advanced | High-performance PyTorch library for GS |
| Trainer + Viewer | **Nerfstudio** | trainer, desktop, open-source, framework | Yes | Windows / Linux | Images / Video / COLMAP | `.ply` | Low | Low | Advanced | Modular framework |
| Trainer | **OpenSplat** | trainer, desktop, open-source | Yes | Windows / Linux | Images (COLMAP / Nerfstudio) | `.ply` | Medium | Medium | Advanced | C++ native GS implementation based on LibTorch |
| Trainer + Viewer | **LichtFeld Studio** | trainer, desktop, open-source | Yes | Windows / Linux | Images / Video / COLMAP | `.ply`, `.spz`, `.lfs` | High | Medium | Advanced | Optimized C++ trainer |

</details>

---

## Observations

## Luma AI
- Fully automated cloud-based training pipeline.
- Scenes are stored in proprietary Luma Field format optimized for real-time rendering.
- Export to `.ply` is available.
- Scenes can be explored interactively through the web interface.

## Teleport
- Cloud reconstruction platform designed for spatial visualization.
- Allows interactive navigation of reconstructed environments in browser.
- `.ply` export available only in paid plans.


## Polycam
- Mobile and web capture platform supporting photogrammetry and Gaussian Splatting workflows.
- Models can be previewed and navigated interactively online.
- Point-cloud export formats (including `.ply`) depend on subscription tier.


## Postshot
- Local desktop Gaussian Splatting training with configurable reconstruction parameters.
- Includes interactive scene viewing after training.
- `.ply` export available only in paid plans.

## Scaniverse
- On-device Gaussian Splatting training performed locally on mobile device.
- Scenes can be interactively explored on mobile and in VR applications.

## Inria gaussian-splatting
- Reference research implementation of the original method.

## gsplat
- CUDA/PyTorch library implementing Gaussian Splatting operations.
- Serves as backend for higher-level frameworks (e.g., Nerfstudio).

## Nerfstudio
- Modular training framework for neural fields.
- Includes web viewer for real-time monitoring and scene navigation.
- Uses `gsplat` as CUDA backend for Splatfacto.

## OpenSplat
- Standalone C++ implementation using LibTorch.
- Removes Python dependencies for portability.
- Supports multiple dataset formats (COLMAP, Nerfstudio, OpenMVG).

## LichtFeld Studio
- Standalone C++ training suite with integrated real-time visualization.
- Provides GUI for monitoring training and navigating the reconstructed scene.

---

## Sources

## Luma AI
- https://lumalabs.ai/interactive-scenes

## Teleport (Varjo)
- https://teleport.varjo.com/
- https://teleport.varjo.com/docs/

## Polycam
- https://poly.cam/tools/gaussian-splatting
- https://learn.poly.cam/hc/en-us/articles/30549121659412-How-to-Create-Photogrammetry-Models-From-Images-and-Videos-on-Polycam-Web

## Postshot (Jawset)
- https://www.jawset.com/
- https://www.jawset.com/docs/d/Postshot+User+Guide
- https://www.youtube.com/watch?v=ERuRMOVO58Q

## Scaniverse
- https://scaniverse.com/
- https://scaniverse.com/news/intro-gaussian-splats
- https://scaniverse.com/news/scaniverse-introduces-support-for-3d-gaussian-splatting

## Inria gaussian-splatting
- https://github.com/graphdeco-inria/gaussian-splatting

## gsplat
- https://github.com/nerfstudio-project/gsplat

## Nerfstudio
- https://github.com/nerfstudio-project/nerfstudio
- https://docs.nerf.studio/index.html

## OpenSplat
- https://github.com/pierotofy/OpenSplat

## LichtFeld Studio
- https://github.com/MrNeRF/LichtFeld-Studio
- https://lichtfeld.io/

---
