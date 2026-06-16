# gsplat — How-To (Training, Export, and Visualization)

This document describes the workflow used for the **gsplat** implementation, including:

- environment setup and installation (Windows)
- dataset preparation  
- training  
- export for downstream inspection and cleaning  

---

## References

- **gsplat repository**:  
  https://github.com/nerfstudio-project/gsplat  

- **Windows installation guide**:  
  https://github.com/nerfstudio-project/gsplat/blob/main/docs/INSTALL_WIN.md

- **Official documentation**:  
 https://docs.gsplat.studio/main/ 

- **Windows-oriented fork**:  
  https://github.com/jonstephens85/gsplat_3dgut  

- **Windows tutorial video**:  
  https://www.youtube.com/watch?v=ACPTiP98Pf8  

For this work, the Windows-oriented fork was used as an installation reference and the tutorial video was followed for the complete setup workflow.  
Both resources are maintained by the same author.

---

## 1. Overview

The `gsplat` project is a research implementation providing CUDA-accelerated Gaussian Splatting operators.

The library does not estimate camera poses.  
The provided training examples expect a calibrated multi-view dataset with known camera parameters.

The workflow consists of:

1. Provide a calibrated dataset (images + camera parameters)
2. Run a training script (e.g., `examples/simple_trainer.py`)
3. Export the trained reconstruction

---

## 2. Prerequisites

- NVIDIA GPU with **CUDA** support  
- CUDA Toolkit compatible with the installed PyTorch version (tested with CUDA 11.8 and PyTorch 2.7.1)  
- Python 3.8+ (Conda/Miniconda recommended) (tested with Python 3.10.19)  
- Git  

### 2.1 Windows-specific notes

The Windows fork and the tutorial video provide guidance for:

- installing the CUDA Toolkit (CUDA 11.8 or 12.6)  
- ensuring CUDA + PyTorch compatibility  
- Visual Studio setup (VS2019 for compatibility with CUDA 11.8)
- installing COLMAP  
- installing ImageMagick for optional image downsampling   

---

## 3. Create the Environment

Using Conda:

```bash
conda create --name gsplat -y python=3.10
conda activate gsplat
```

Setup Visual Studio as shown in the video and as explained in the Windows-oriented fork.  

Install the CUDA Toolkit and then PyTorch matching CUDA as shown in the video and as explained in the Windows-oriented fork.

Example for CUDA 11.8:

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

---

## 4. Install gsplat and Requirements

From the repository root:

```bash
pip install -e .
pip install -r examples/requirements.txt
```

---

## 5. Replace scene_manager.py File

In order to use gsplat on Windows, it is required to replace the `scene_manager.py` file.  
It can be downloaded here:

https://github.com/jonstephens85/gsplat_3dgut/blob/main/assets/scene_manager.py

The file to replace can be found in:

```text
C:\Users\<username>\anaconda3\envs\gsplat\Lib\site-packages\pycolmap
```

---

## 6. Optional Dataset Preparation from Video (ffmpeg)

If the dataset is provided as a video rather than a set of images, frames must first be extracted.  
This can be done using `ffmpeg`, as shown in the **Inria GS Windows tutorial video**:

https://www.youtube.com/watch?v=UXtuigy_wYc

Frames were extracted from input videos using `ffmpeg`.

Run the following command from the directory containing the input video:

```bash
ffmpeg -i {VIDEO} -qscale:v 1 -qmin 1 -vf fps={FPS} %04d.jpg
```

Where:

- `{VIDEO}` should be replaced with the filename or full path to the input video file  
- `{FPS}` should be replaced with the desired frame extraction rate  

The extracted frames will be saved as sequentially numbered `.jpg` images (e.g., `0001.jpg`, `0002.jpg`, ...).

---

### 6.1 FPS Selection Heuristic

The extraction FPS was chosen so as to obtain **approximately 150 frames per scene** (**151 frames in the final benchmark datasets**).

---

## 7. COLMAP

The training scripts require a calibrated dataset with known camera parameters.

Typical structure:

```text
<dataset>
|---images
|---sparse
    |---0
        |---cameras.bin
        |---images.bin
        |---points3D.bin
```

The dataset can be generated using any photogrammetry software (e.g., COLMAP).  
Camera estimation is not part of the gsplat training pipeline.

COLMAP is software used to perform SfM (Structure-from-Motion).  
It can be used either via the terminal or with its GUI.

Usage via terminal is explained in the Windows-oriented fork.  
Usage with the GUI is explained both in the Windows-oriented fork and shown in the tutorial video.

---

## 8. Training

Training is performed using the example trainer script.

Generic command:

```bash
python examples/simple_trainer.py {CONFIG} --data-dir {DATA_PATH} --result-dir {OUTPUT_DIR}
```

Training was performed using the `examples/simple_trainer.py` script provided in the gsplat repository.

Although the repository also provides an `mcmc` configuration (demonstrated in the tutorial video and Windows-oriented fork), the **standard (`default`) configuration was used for both benchmark experiments**.

---

### 8.1 Default Configuration (Used in Benchmark)

Example:

```bash
python examples/simple_trainer.py default \
    --data-dir data/video-esterno \
    --data-factor 2 \
    --result-dir results/video-esterno \
    --camera-model pinhole \
    --max-steps 30000 \
    --save-ply
```

#### Parameter description

- `default` → standard training configuration  
- `--data-dir` → path to the dataset directory  
- `--data-factor` → image downsampling factor (typically 1, 2, 4, or 8). The image resolution is divided by this factor per dimension (e.g., `2` → 1/2 width and height; `4` → 1/4 width and height).
- `--result-dir` → output directory  
- `--camera-model pinhole` → --camera-model → projection model used during training and rendering 
- `--max-steps 30000` → number of optimization steps  
- `--save-ply` → exports the final reconstruction as a `.ply` file

The `--camera-model` parameter can be set to:

- `pinhole` → standard perspective camera model (regular camera)  
- `fisheye` → fisheye lens model for wide-angle cameras  

In this benchmark, `pinhole` was used to match the projection model of the dataset cameras.

---

### 8.2 Note on MCMC Configuration

The `mcmc` configuration is also available in the repository and is demonstrated in the tutorial resources.  
However, it was **not used in the benchmark experiments**.

---

## 8. Export

By using `--save-ply` in the training command, the final reconstruction is exported as a `.ply` file.

---
