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

### 1. The listed tools cover multiple stages of the Gaussian Splatting workflow
They do not represent a single software category but different operational phases:

- **Inspection/Editing**: **SuperSplat** allows editing and inspecting splats directly in the browser.
- **Training monitoring**: **Nerfstudio Viewer** and the remote mode of **SIBR** connect to a running optimization process.
- **Model visualization**: **SIBR** real-time viewer and **Unity-based tools** display already trained models.
- **Rendering integration**: ***UnityGaussianSplatting  (aras-p)** and **Unity-VR-Gaussian-Splatting (ninjamode)** provide a rendering package intended to be integrated into projects.
- **Immersive visualization**: **Unity-VR-Gaussian-Splatting (ninjamode)** and **GaussianSplattingVRViewerUnity (clarte53)** enable visualization inside XR environments.

### 2. Training viewers rely on streaming rather than saved models
**Nerfstudio Viewer** and the remote **SIBR** viewer connect to the optimization process and receive continuous updates instead of loading a trained model from disk. **Playback viewers** instead operate on exported model files. 

### 3. SIBR uniquely supports both training monitoring and model playback
SIBR provides two distinct viewers: one connected to the optimizer and another for viewing trained models. 

### 4. **Unity-VR-Gaussian-Splatting (ninjamode)** is a fork of **UnityGaussianSplatting (aras-p)**
While **UnityGaussianSplatting (aras-p)** is a rendering package integrated into Unity projects, **Unity-VR-Gaussian-Splatting (ninjamode)** extends its functionalities to VR rendering support.

### 5. GaussianSplattingVRViewerUnity (clarte53) provides a VR viewer
Unlike the other Unity-based tools, which are distributed as development projects, **GaussianSplattingVRViewerUnity (clarte53)** provides an executable VR viewer application.

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
