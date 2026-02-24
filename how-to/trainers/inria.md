# Inria gaussian-splatting — How-To (Training, Export, and Visualization)

This document describes the workflow used for the **Inria Gaussian Splatting** implementation, including:

- environment setup and installation (Windows)
- dataset preparation from video  
- dataset conversion  
- training  
- visualization with SIBR (Windows)  
- export for downstream inspection and cleaning

---

## References

- **Official Inria gaussian-splatting implementation**:  
  https://github.com/graphdeco-inria/gaussian-splatting

- **Website**:  
  https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/

- **SIBR Website**:        
  https://sibr.gitlabpages.inria.fr/?page=index.html&version=0.9.6

- **Windows-oriented fork + install guide**:  
  https://github.com/jonstephens85/gaussian-splatting-Windows

- **Windows tutorial video**:  
  https://www.youtube.com/watch?v=UXtuigy_wYc

For this work, the Windows-oriented fork was used as installation reference and the tutorial video was followed for the complete setup workflow.  
Both resources are maintained by the same author.

---

## 1. Overview

The Inria gaussian-splatting implementation is the **reference research implementation of 3D Gaussian Splatting**.

The pipeline operates on calibrated camera poses.  
If they are not provided, the `convert.py` script can be used to automatically run a Structure-from-Motion reconstruction using COLMAP.

The workflow is composed of two main stages:

1. Dataset preparation (`convert.py`)
2. Optimization (`train.py`)

The SIBR viewer is a separate real-time renderer used only for visualization.

---

## 2. Prerequisites

- NVIDIA GPU with **CUDA** support  
- CUDA Toolkit compatible with your PyTorch build (tested with CUDA 11.8 and PyTorch 1.12.1)
- Python 3.8+ (Conda/Miniconda recommended) (tested with Python 3.8.20)
- Git  

### 2.1 Windows-specific notes

The Windows fork and the tutorial video provide step-by-step guidance for:

- installing the CUDA toolkit (CUDA 11.8)
- ensuring CUDA + PyTorch compatibility  
- Visual Studio setup (VS2019 for compatibility with CUDA 11.8)  
- installing COLMAP  
- installing ImageMagick for image preparation  
- installing FFmpeg to extract images from videos  
- building CUDA extensions if `pip install` fails  
- downloading and using the SIBR pre-built binaries for Windows  
- common Windows build/runtime issues  

---

## 3. Clone the Repository

Always clone the official repository **with submodules**:

```bash
git clone https://github.com/graphdeco-inria/gaussian-splatting.git --recursive
cd gaussian-splatting
```

---

## 4. Create the Environment

Using Conda:

```bash
SET DISTUTILS_USE_SDK=1  # Windows only
conda env create --file environment.yml
conda activate gaussian_splatting
```

---

## 5. Optional Dataset Preparation from Video (ffmpeg)

If the dataset is provided as a video rather than a set of images, frames must first be extracted.  
This can be done using `ffmpeg`, as shown in the Windows tutorial video.

Run the following command from the directory containing the input video:

```bash
ffmpeg -i {VIDEO} -qscale:v 1 -qmin 1 -vf fps={FPS} %04d.jpg
```

Where:

- `{VIDEO}` should be replaced with the filename or full path to the input video file  
- `{FPS}` should be replaced with the desired frame extraction rate  

The extracted frames will be saved as sequentially numbered `.jpg` images (e.g., `0001.jpg`, `0002.jpg`, ...).

### 5.1 FPS Selection Heuristic

The extraction FPS was chosen so as to obtain **approximately 150 frames per scene** (**151 frames in the final benchmark datasets**).

---

## 6. Input Dataset

The Inria implementation supports two dataset preparation modes.

### 6.1 Precalibrated Dataset

The training pipeline can directly consume an existing SfM reconstruction.

Expected structure:

```text
<location>
|---images
|---sparse
    |---0
        |---cameras.bin
        |---images.bin
        |---points3D.bin
```

In this mode, camera poses and intrinsics are already known and the Gaussian Splatting optimization runs directly on the calibrated dataset.

---

### 6.2 Automatic Reconstruction from Images

If only images are available, the `convert.py` script can automatically run COLMAP and prepare the dataset.

Place images inside:

```text
<location>
|---input
    |---<image 0>
    |---<image 1>
    |---...
```

Then run:

```bash
python convert.py -s {DATA_PATH}
```

Example:

```bash
python convert.py -s data\video-interno
```

---

## 7. Training

From the repository root:

```bash
python train.py -s {DATA_PATH}
```

Example:

```bash
python train.py -s data\video-interno
```

Training outputs and checkpoints are written under the `output/` directory.

---

## 8. Visualization with SIBR (Windows)

The Inria implementation provides a real-time OpenGL viewer called **SIBR**.

After downloading the pre-compiled Windows build as shown in the tutorial video, the SIBR viewer binaries were copied into the root directory of the `gaussian-splatting` repository.  
The resulting executable path is:

```text
<GAUSSIAN_SPLATTING_ROOT>\viewers\bin\SIBR_gaussianViewer_app.exe
```

To visualize a trained scene, move to the `viewers\bin` directory and run:

```bash
SIBR_gaussianViewer_app -m {DATA_PATH}
```

where `-m` points to the output directory generated by the training run.

Example:

```bash
SIBR_gaussianViewer_app -m output\video-interno
```

---

## 9. Export

The exported `.ply` file can be found in the output directory of the training run.

---
