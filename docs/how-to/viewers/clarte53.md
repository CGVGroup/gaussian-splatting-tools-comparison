# GaussianSplattingVRViewerUnity (clarte53) — How-To (Setup and VR Execution)

This document describes the workflow used for **GaussianSplattingVRViewerUnity (clarte53)**, including:

- obtaining and launching a viewer release
- importing and visualizing Gaussian Splatting reconstructions in VR

---

## References

- **GaussianSplattingVRViewerUnity (clarte53) repository**:  
  https://github.com/clarte53/GaussianSplattingVRViewerUnity

- **Pre-built releases**:  
  https://github.com/clarte53/GaussianSplattingVRViewerUnity/releases

---

## Important Note

For the experiments, a **precompiled Windows binary** was used.

The procedure described below refers to the precompiled version.

It is also possible to compile the project from source.

---

## 1. Overview

GaussianSplattingVRViewerUnity (clarte53) is a Unity-based VR viewer application designed to load and render Gaussian Splatting `.ply` reconstructions inside a VR environment.

The workflow consists of:

1. Download and install a release build
2. Configure the VR runtime
3. Load a Gaussian Splatting `.ply` file
4. Navigate and inspect the reconstruction in VR

---

## 2. Requirements

Hardware requirements:

- MS Windows VR Ready computer
- Minimal GPU: CUDA-ready GPU with Compute Capability 7.0+ (GeForce RTX 2060 or higher)
- Recommended GPU: GeForce RTX 4070 or higher

---

## 3. Installation (Prebuilt Version Used)

For the experiments, a **precompiled Windows binary** was used.

Download from:

https://github.com/clarte53/GaussianSplattingVRViewerUnity/releases

After download:

1. Extract the archive.
2. Open the executable folder.

At this stage, you have a VR viewer executable.

---

## 4. Importing and Viewing a Reconstruction

Move the `.ply` files you want to render into the executable folder.

Then, with the headset connected to the computer, run the executable.

The viewer UI should be visible in VR.

---

## 5. Viewer Interaction and Navigation

In the viewer UI, it is possible to select which `.ply` file to render.

The controllers can be used to rotate the view, translate the scene, zoom, or move closer to objects.

---
