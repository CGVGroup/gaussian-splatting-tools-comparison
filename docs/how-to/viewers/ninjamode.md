# Unity-VR-Gaussian-Splatting (ninjamode) — How-To (Setup and VR Execution)

This document describes the workflow used for **Unity-VR-Gaussian-Splatting (ninjamode)**, including:

- Unity project setup
- project configuration
- model import
- Gaussian Splatting Asset usage

---

## References

- **Unity-VR-Gaussian-Splatting (ninjamode) repository**:  
  https://github.com/ninjamode/Unity-VR-Gaussian-Splatting

---

## 1. Overview

Unity-VR-Gaussian-Splatting (ninjamode) is a Unity-based rendering package that extends the original UnityGaussianSplatting (aras-p) implementation by adding **Virtual Reality (VR) support**.

The workflow consists of:

1. Open the Unity project
2. Import a `.ply` Gaussian Splatting model
3. Configure XR settings
4. Run in PCVR mode
5. Inspect the reconstruction in VR

---

## 2. Prerequisites

- The repository works with: HTC Vive series headsets, Varjo Aero, Quest Pro, Quest 3
- In this work, it was tested with the Unity Editor Version 2022.3.47f1

---

## 3. Clone the Repository

```bash
git clone https://github.com/ninjamode/Unity-VR-Gaussian-Splatting.git
```

Open the project using **Unity Hub**.

---

## 4. Scene Preparation

XR configuration was performed via **Project Settings → XR Plug-in Management**, enabling **OpenXR**.

Import the `.ply` files into the Unity project as Assets using: **Tools → GaussianSplats → Create Gaussian Splat Asset**.

During import, the asset quality level can be selected from the available presets: **Very Low, Low, Medium, High, Very High**.

In a Unity scene properly configured for XR (including an XR Origin for headset tracking), create an empty GameObject and add the **Gaussian Splat Renderer** component. Assign the Gaussian Splat Asset to its Asset field in order to render the reconstruction inside the VR environment.

---

## 5. Using Gaussian Splatting Reconstructions inside the Unity Project

After completing the steps above, the imported Gaussian Splatting Asset can be rendered directly inside the Unity scene through the **Gaussian Splat Renderer** component and behaves as a scene object.

---
