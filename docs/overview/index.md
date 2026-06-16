# Overview

This section provides descriptive documentation of tools in the Gaussian Splatting ecosystem. It does not include controlled benchmarking — its purpose is to map the landscape and provide context before diving into the experimental analysis.

---

## What are trainers?

Trainers are the pipelines that take a set of calibrated images or video frames and produce a Gaussian Splatting representation (a `.ply` file or equivalent). They differ in architecture, language, platform, speed, and output density.

The [Trainers overview](trainers.md) covers five open-source and five commercial options, including qualitative notes on their characteristics and trade-offs.

---

## What are viewers?

Viewers are tools used to inspect, edit, and visualize Gaussian Splatting models after training. They span browser-based editors, desktop playback tools, live training monitors, and immersive XR environments.

The [Viewers overview](viewers.md) covers six tools across these categories, from the SuperSplat web editor to Unity-based VR viewers.

---

## Relation to the experimental analysis

The tools documented here that were selected for controlled evaluation are:

- **Trainers**: Inria gaussian-splatting, gsplat, OpenSplat, Nerfstudio, LichtFeld Studio → [Trainer benchmark](../analysis/trainers/methodology.md)
- **Viewers**: Unity-VR-Gaussian-Splatting (ninjamode), GaussianSplattingVRViewerUnity (clarte53) → [XR viewer analysis](../analysis/viewers/methodology.md)
