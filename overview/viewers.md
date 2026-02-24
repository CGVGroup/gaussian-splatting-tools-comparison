# Gaussian Splatting Viewers Overview

This document provides a qualitative overview of Gaussian Splatting viewers.

## Overview Table

| Name | Tags | Platform | Execution environment | Role | Model access | Entry Guide | Notes |
|------|------|------|------|------|------|------|------|
| SuperSplat | editor, offline, web-based | Web | Browser | Editor & playback viewer | Local files | Easy | No installation, drag-and-drop editing and inspection |
| Nerfstudio Viewer | training, streaming, web-based | Web | Browser | Live training viewer | Live pipeline stream | Medium | Requires running training pipeline locally |
| SIBR Viewer | training, offline, desktop | Desktop | Standalone application | Training monitoring and trained-model viewing | Network stream or local model | Medium | Needs compiled binaries and dataset preparation |
| UnityGaussianSplatting (aras-p) | renderer, unity | Desktop | Unity engine | Gaussian Splatting rendering package for Unity | Local assets | Advanced | Integration inside Unity project required |
| Unity-VR-Gaussian-Splatting (ninjamode) | xr, renderer, unity | VR | Unity XR | Gaussian Splatting Virtual Reality rendering package for Unity | Local assets | Advanced | Integration inside Unity project required |
| GaussianSplattingVRViewerUnity (clarte53) | xr, application, unity | VR | Unity XR | VR viewer application for gaussian splatting models | Local assets | Medium | Provides executable VR viewer and Unity project |

---

## Observations

- The tools can be grouped by stage of the Gaussian Splatting workflow:
  - **Inspection / Editing**: SuperSplat enables browser-based splat inspection and editing.
  - **Training monitoring**: Nerfstudio Viewer and the remote SIBR viewer connect to a running optimization process.
  - **Model visualization**: SIBR real-time viewer and Unity-based tools display already trained models.
  - **Rendering integration**: UnityGaussianSplatting (aras-p) and Unity-VR-Gaussian-Splatting (ninjamode) provide rendering packages designed for integration into Unity projects.
  - **Immersive visualization**: Unity-VR-Gaussian-Splatting (ninjamode) and GaussianSplattingVRViewerUnity (clarte53) enable visualization inside XR environments.

- **SIBR Viewer** is distributed as part of the official Inria / GraphDeco gaussian-splatting repository and represents the reference visualization tool for models trained with the original Inria implementation.

- Training viewers (**Nerfstudio Viewer** and remote **SIBR**) rely on live streaming from the optimization pipeline rather than loading saved model files.

- Playback viewers operate on exported model files and do not require a running training process.

- **SIBR** uniquely supports both training monitoring (remote mode) and trained-model playback (real-time viewer).

- **Unity-VR-Gaussian-Splatting (ninjamode)** is a fork of **UnityGaussianSplatting (aras-p)**, extending the original rendering package with VR support.

- **GaussianSplattingVRViewerUnity (clarte53)** provides a VR viewer application.

---

## Sources

## SuperSplat
- https://github.com/playcanvas/supersplat

## Nerfstudio Viewer
- https://docs.nerf.studio/quickstart/viewer_quickstart.html

## SIBR Viewer (GraphDeco)
- https://github.com/graphdeco-inria/gaussian-splatting

## UnityGaussianSplatting (Aras-p)
- https://github.com/aras-p/UnityGaussianSplatting

## Unity-VR-Gaussian-Splatting (Ninjamode)
- https://github.com/ninjamode/Unity-VR-Gaussian-Splatting

## GaussianSplattingVRViewerUnity (Clarte53)
- https://github.com/clarte53/GaussianSplattingVRViewerUnity
- https://github.com/clarte53/GaussianSplattingVRViewerUnity/blob/master/GaussianSplattingVRViewer/README.md

---
