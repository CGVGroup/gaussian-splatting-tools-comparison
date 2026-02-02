# Gaussian Splatting Trainers – Comparative Table

This document provides a qualitative overview of Gaussian Splatting training pipelines, including both commercial and open-source tools.  
Tools are grouped using collapsible sections for readability in GitHub.

---

## 📦 Commercial Trainers

<details open>
<summary><strong>Show / Hide Commercial Trainers</strong></summary>

<br>

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer + Viewer | **Luma AI** | trainer, online, commercial, web-view | No | Cloud | Video / Images | Proprietary, `.ply` (mesh / point cloud export) | Low | Medium | Easy | Fully automated cloud-based pipeline |
| Trainer + Viewer | **Teleport** | trainer, online, commercial, immersive-view | No | Cloud | Video / Images | Proprietary | Variable | Medium | Easy | Desktop and VR-oriented cloud-based pipeline |
| Trainer + Viewer | **Polycam** | trainer, online, mobile, commercial, web-view | No | iOS / Android / Cloud | Video / Images | Multiple 3D formats | Medium | Low | Easy | Mobile and cloud-enabled GS pipeline |
| Trainer + Viewer | **Postshot** | trainer, desktop, commercial, desktop-view | No | Windows | Images / Video | `.ply`, `.spz`, `.psht` | High | Medium | Medium | Local desktop GS training |
| Trainer + Viewer | **Scaniverse** | trainer, mobile, commercial, immersive-view | No | iOS / Android | Video | `.ply`, `.spz` | Medium | Low | Easy | On-device GS capture and VR viewing |

</details>

---

## 🧪 Open-Source Trainers

<details open>
<summary><strong>Show / Hide Open-Source Trainers</strong></summary>

<br>

| Tool Type | Name | Tags | Open Source | Platform | Input | Output Format | Output Size | Processing Time | Entry Guide | Notes |
|----------|------|------|-------------|----------|-------|---------------|-------------|-----------------|-------------|------|
| Trainer | **Inria Gaussian Splatting** | trainer, desktop, open-source | Yes | Windows / Linux | Images | `.ply` | High | High | Medium | Reference open-source implementation |
| Trainer | **gsplat** | trainer, desktop, open-source | Yes | Windows / Linux | Images | `.ply` | High | Medium | Medium | High-performance PyTorch library for GS |
| Trainer + Viewer | **Nerfstudio** | trainer, desktop, open-source, framework | Yes | Windows / Linux | Images / Video / COLMAP | `.ply` | Low | Low | Medium | Modular framework |
| Trainer | **OpenSplat** | trainer, desktop, open-source | Yes | Windows / Linux | Images (COLMAP / Nerfstudio) | `.ply` | Medium | Medium | Medium | C++ native GS implementation based on LibTorch |
| Trainer + Viewer | **LichtFeld Studio** | trainer, desktop, open-source | Yes | Windows / Linux | Images / Video / COLMAP | `.ply`, `.spz`, `.lfs` | High | Medium | Medium | Optimized C++ trainer |

</details>

---

# Notes on the Trainers Table

## Luma AI
- The training pipeline is fully automated and cloud-based. No user-accessible training parameters are exposed.
- Scenes are internally stored as proprietary **Luma Field** assets optimized for real-time rendering.
- Export to standard `.ply` formats (mesh or point cloud) is supported.
- An official **Unreal Engine plugin** is available for real-time visualization of Luma Fields.

## Teleport
- Designed primarily for immersive and VR-oriented visualization.
- Free plan allows up to **5 models**; **`.ply` export** only in paid plan.
- Supports **lightweight (0.25 M)** models for fast previews and **full (~2 M)** models with **LOD** for immersive VR viewing.

## Polycam
- Mobile 3D capture app (iOS/Android) for photogrammetry, LiDAR scans, and 360° captures, with on-device editing and sharing.  
- Cloud/online platform for managing, previewing, editing, and exporting models in formats like GLTF, OBJ, FBX, STL, PLY.  
- Free plan allows limited captures and basic export; paid plans unlock `.splat` export and higher limits.  
- Models can be shared via links, synced across devices, or viewed in community/Poly World feeds.

## Postshot
- Enables local desktop training with configurable reconstruction settings.
- Supports images of any resolution (up to 4K/8K), video inputs, image masking, ACES color space, and geo-referenced data.  
- Can import camera poses/points from 3rd-party tools and focus training on region-of-interest.  
- Models in this work were generated with the **Model Profile Splat3** using **Limit Image Size → 1600px**, **Image Selection → Use Best Images**, **Image Count → 150**, and **Camera poses → Compute From Images**.
- Provides a command-line interface.
- Starting from Postshot v1.0, exporting Gaussian Splatting models in `.ply` format is no longer available under the free license; export options are restricted to paid plans.

## Scaniverse
- Users can explore and interact with splats on mobile and through related VR/WebXR experiences like Into The Scaniverse on Meta Quest.

## Inria Gaussian Splatting
- Reference open-source implementation of Gaussian Splatting introduced in the original research work.
- Provides full control over training parameters, optimization schedule, and data processing.
- Relies on external Structure-from-Motion pipelines (e.g., COLMAP) for camera pose estimation.
- Intended primarily for research, benchmarking, and reproducibility rather than end-user workflows.

## gsplat
- A high-performance, open-source library for Gaussian Splatting, designed for modular research and experimentation.
- Acts as the core backend for several high-level frameworks, including **Nerfstudio**.
- Features highly optimized CUDA kernels and advanced densification strategies such as **MCMC (Markov Chain Monte Carlo)**.
- Built on **PyTorch**, it offers superior training speeds and memory efficiency compared to the original Inria reference code.

## Nerfstudio
- A modular framework designed to simplify the end-to-end process of creating, training, and visualizing neural fields.
- Implements **Splatfacto**, a standardized Gaussian Splatting model that uses the **gsplat** library as its high-performance CUDA backend.
- Features an integrated **Web Viewer (Viser)** for real-time monitoring of the training process, allowing interactive inspection of densification and rendering parameters.
- Supports **Camera Pose Refinement**, optimizing camera extrinsics during training to improve reconstruction accuracy from SfM data.
- Provides a flexible pipeline for data processing and training.

## OpenSplat
- A standalone **C++ implementation** of the Gaussian Splatting pipeline using **LibTorch**.
- Key advantage is the **removal of Python dependencies**, resulting in a portable executable suitable for production environments.
- Strictly follows the original Inria optimization logic while offering a more lightweight architecture.
- Provides native support for multiple project formats including **COLMAP**, **Nerfstudio**, and **OpenMVG**.

## LichtFeld Studio
- A high-performance, **standalone C++ Gaussian Splatting suite** that integrates training and real-time visualization in a single desktop application.
- It is the reference implementation for **MCMC (Markov Chain Monte Carlo)** densification strategies, which allow for more efficient point distribution and cleaner reconstructions.
- Designed as a **portable application** with minimal dependencies, providing a specialized GUI for monitoring training metrics and real-time rendering quality.
