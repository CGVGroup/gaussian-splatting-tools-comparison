# OpenSplat — How-To (Training and Export)

This document describes the workflow used for the **OpenSplat** implementation, including:

- environment setup (Windows)
- native dependencies
- dataset preparation
- training
- export for downstream inspection and cleaning

---

## References

- **OpenSplat repository**: https://github.com/pierotofy/OpenSplat

Build requirements are documented in the official OpenSplat repository through dependency specifications and CMake configuration files.

---

## 1. Prerequisites

- NVIDIA GPU with **CUDA** support
- CUDA Toolkit (tested with CUDA 11.8)
- CMake
- LibTorch (C++ PyTorch distribution) (tested with LibTorch 2.1.2)
- OpenCV (tested with OpenCV 4.9.0)
- Git

### 1.1 Windows-specific notes

- Visual Studio (tested with VS2019)

OpenSplat is a native C++/CUDA application and does not rely on a Python environment.

The build requires a compatible Visual Studio and CUDA toolchain.  
In this setup, Visual Studio 2019 was used together with CUDA 11.8.

---

## 2. Clone the Repository

Download the OpenSplat repository:

```bash
git clone https://github.com/pierotofy/OpenSplat.git
cd OpenSplat
```

---

## 3. Setup

The project was compiled locally using CMake and Visual Studio.

The OpenSplat repository provides dependency requirements and CMake configuration files describing the build procedure.

The environment was prepared in the following order:

1. OpenSplat repository download
2. CUDA Toolkit installation
3. LibTorch download and extraction
4. OpenCV installation/build
5. CMake configuration and Visual Studio build

The compilation produced the `opensplat.exe` executable used in the benchmark experiments.

---

## 4. Dataset

OpenSplat requires an existing reconstruction project (e.g., COLMAP, OpenSfM, ODM, or Nerfstudio).

The dataset was generated using **COLMAP** from extracted video frames.  
The expected folder structure is described in **Section 5.2 (Dataset Structure)**.

---

## 5. Dataset Preparation from Video (ffmpeg)

If the dataset is provided as a video rather than a set of images, frames must first be extracted.
This can be done using `ffmpeg`, as shown in the **Inria GS Windows tutorial video**:

https://www.youtube.com/watch?v=UXtuigy_wYc

Run the following command from the directory containing the input video:

```bash
ffmpeg -i {video} -qscale:v 1 -qmin 1 -vf fps={fps} %04d.jpg
```

Where:

- `{video}` should be replaced with the filename or full path to the input video file
- `{fps}` should be replaced with the desired frame extraction rate

The extracted frames will be saved as sequentially numbered `.jpg` images (e.g., `0001.jpg`, `0002.jpg`, ...).

### 5.1 FPS Selection Heuristic

The extraction FPS was chosen so as to obtain **approximately 150 frames per scene** (**151 frames in the final benchmark datasets**).

### 5.2 Dataset Structure

**COLMAP** was used to generate the sparse reconstruction from the extracted frames.

Therefore the reconstruction project had the following structure:

```text
<location>
|---images
|   |---<image 0>
|   |---<image 1>
|   |---...
|---sparse
    |---0
        |---cameras.bin
        |---images.bin
        |---points3D.bin
```

---

## 6. Training

Training is performed directly through the OpenSplat executable.

Example:

```bash
opensplat.exe -n 30000 -s 5000 -d 2 -o "C:\Users\ernes\OpenSplat_Project\OpenSplat\results\video-interno.ply" "C:\Users\ernes\OpenSplat_Project\OpenSplat\data\video-interno"
```

### Parameter description

- `-n 30000` → number of optimization iterations (30k steps)
- `-s 5000` → checkpoint interval
- `-d 2` → image downsampling factor
- `-o` → output `.ply` file
- final argument → dataset directory

---

### 6.1 Downsampling Factor

The `-d` parameter controls image resolution during training:

- `-d 1` → original resolution
- `-d 2` → half resolution per dimension (¼ total pixels)
- `-d 4` → quarter resolution
- `-d 8` → eighth resolution

---

## 7. Export

The reconstruction is exported directly as a `.ply` file using the `-o` flag.

---
