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

- **Windows-oriented fork**:  
  https://github.com/jonstephens85/gsplat_3dgut  

- **Windows tutorial video**:  
  https://www.youtube.com/watch?v=ACPTiP98Pf8  

For this work, the Windows-oriented fork was used as an installation reference and the tutorial video was followed for the complete setup workflow.  
Both resources are maintained by the same author.

---

## 1. Prerequisites

- NVIDIA GPU with **CUDA** support  
- CUDA Toolkit compatible with the installed PyTorch version (tested with CUDA 11.8 and PyTorch 2.7.1)  
- Python 3.8+ (Conda/Miniconda recommended) (tested with Python 3.10.19)  
- Git  

### 1.1 Windows-specific notes

The Windows fork and the tutorial video provide guidance for:

- installing the CUDA Toolkit (CUDA 11.8 or 12.6)  
- ensuring CUDA + PyTorch compatibility  
- Visual Studio setup (VS2019 for compatibility with CUDA 11.8)
- installing COLMAP  
- installing ImageMagick for image preparation      

---

## 2. Create the Environment

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

## 3. Install gsplat and Requirements

From the repository root:

```bash
pip install -e .
pip install -r examples/requirements.txt
```

---

## 4. Replace scene_manager.py File

In order to use gsplat on Windows, it is required to replace the `scene_manager.py` file.  
It can be downloaded here:

https://github.com/jonstephens85/gsplat_3dgut/blob/main/assets/scene_manager.py

The file to replace can be found in:

```text
C:\Users\<username>\anaconda3\envs\gsplat\Lib\site-packages\pycolmap
```

---

## 5. Dataset Preparation from Video (ffmpeg)

If the dataset is provided as a video rather than a set of images, frames must first be extracted.  
This can be done using `ffmpeg`, as shown in the **Inria GS Windows tutorial video**:

https://www.youtube.com/watch?v=UXtuigy_wYc

Frames were extracted from input videos using `ffmpeg`.

Run the following command from the directory containing the input video:

```bash
ffmpeg -i {video} -qscale:v 1 -qmin 1 -vf fps={fps} %04d.jpg
```

Where:

- `{video}` should be replaced with the filename or full path to the input video file  
- `{fps}` should be replaced with the desired frame extraction rate  

The extracted frames will be saved as sequentially numbered `.jpg` images (e.g., `0001.jpg`, `0002.jpg`, ...).

---

### 5.1 FPS Selection Heuristic

The extraction FPS was chosen so as to obtain **approximately 150 frames per scene** (**151 frames in the final benchmark datasets**).

---

## 6. COLMAP

COLMAP is software used to perform SfM (Structure-from-Motion).  
It can be used either via the terminal or with its GUI.

Usage via terminal is explained in the Windows-oriented fork.  
Usage with the GUI is explained both in the Windows-oriented fork and shown in the tutorial video.

---

## 7. Training

Training was performed using the `examples/simple_trainer.py` script provided in the gsplat repository.

Although the repository also provides an `mcmc` configuration (demonstrated in the tutorial video and Windows-oriented fork), the **standard (`default`) configuration was used for both benchmark experiments**.

---

### 7.1 Default Configuration (Used in Benchmark)

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
- `--camera-model pinhole` → camera model used for reconstruction  
- `--max-steps 30000` → number of optimization steps  
- `--save-ply` → exports the final reconstruction as a `.ply` file

**Note:** When using `--data-factor`, image downsampling is handled internally during training.  

The `--camera-model` parameter can be set to:

- `pinhole` → standard perspective camera model (regular camera)  
- `fisheye` → fisheye lens model for wide-angle cameras  

In this benchmark, `pinhole` was used, as the datasets were reconstructed using a standard perspective camera model.

This configuration was selected to maintain a fixed number of optimization steps (30,000), ensuring comparability with the other evaluated Gaussian Splatting pipelines.

---

### 7.2 Note on MCMC Configuration

The `mcmc` configuration is also available in the repository and is demonstrated in the tutorial resources.  
However, it was **not used in the benchmark experiments**.

---

## 8. Export

By using `--save-ply` in the training command, the final reconstruction is exported as a `.ply` file.

---
