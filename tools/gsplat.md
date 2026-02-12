# gsplat — How-To (Training, Export, and Visualization)

This document describes the workflow used for the **gsplat** implementation, including:

- environment setup and installation (Windows)  
- dataset preparation  
- training  
- export for downstream inspection and cleaning  

---

## References

- **Official gsplat repository**: https://github.com/nerfstudio-project/gsplat  

- **Windows installation guide**: https://github.com/nerfstudio-project/gsplat/blob/main/docs/INSTALL_WIN.md  

- **Windows-oriented fork**: https://github.com/jonstephens85/gsplat_3dgut  

- **Windows utorial video**: https://www.youtube.com/watch?v=ACPTiP98Pf8  

For this work, the Windows-oriented fork was used as installation reference and the tutorial video was followed for the complete setup workflow.  
Both resources are maintained by the same author.

---

## 1. Prerequisites

- NVIDIA GPU with **CUDA** support  
- CUDA Toolkit compatible with the installed PyTorch version (tested with Cuda 11.8 and PyTorch 2.7.1)
- Python 3.8+ (Conda/Miniconda recommended) (tested with Python 3.10.19)
- Git  

### 1.1 Windows-specific notes

The Windows fork and the tutorial video provide guidance for:

- installing the CUDA Toolkit (CUDA 11.8 or 12.6)
- ensuring CUDA + PyTorch compatibility
- Visual Studio setup (VS2019 for compatibility with CUDA 11.8)

---

## 2. Clone the Repository

```bash
git clone https://github.com/nerfstudio-project/gsplat.git
cd gsplat
```

---

## 3. Create the Python Environment

Using Conda:

```bash
conda create -n gsplat python=3.10
conda activate gsplat
```

Install PyTorch with CUDA matching your system (example for CUDA 11.8):

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

---

## 4. Install gsplat

From the repository root:

```bash
pip install -e .
```

If installation fails on Windows:

- follow the official Windows guide:  
  https://github.com/nerfstudio-project/gsplat/blob/main/docs/INSTALL_WIN.md  

- consult the supplemental repository for fixes and patches:  
  https://github.com/jonstephens85/gsplat_3dgut  

The tutorial video also walks through the installation process step by step.

---

## 5. Dataset Preparation

Dataset preparation follows the same workflow used for other pipelines in this benchmark:

1. Extract frames from video using `ffmpeg`
2. Generate COLMAP reconstruction
3. Ensure dataset follows the expected structure

Expected dataset structure:

```text
<location>
|---images
|---sparse
    |---0
        |---cameras.bin
        |---images.bin
        |---points3D.bin
```

Frame extraction example:

```bash
ffmpeg -i {video} -qscale:v 1 -qmin 1 -vf fps={fps} %04d.jpg
```

In this benchmark, the FPS was selected so as to obtain approximately 150 frames per scene.

---

## 6. Training

Training is launched from the repository root.

Example command:

```bash
python train.py --data {folder}
```

Example:

```bash
python train.py --data data/video-interno
```

Training outputs are written to the default experiment/output directory defined by the script.

---

## 7. Export

After training completes, the reconstructed Gaussian model can be exported to `.ply`.

The exported `.ply` files were used in this benchmark for:

- raw inspection  
- quantitative evaluation  
- post-processing and cleaning in SuperSplat  

---

## 8. Notes on This Benchmark Workflow

- The official Windows installation guide (`INSTALL_WIN.md`) was followed.
- The supplemental `gsplat_3dgut` repository was used when needed to resolve build issues.
- The tutorial video was used as practical guidance.
- Training time measurements include only the automatic optimization phase.
- Exported `.ply` files were processed using the same inspection and cleaning pipeline as other implementations.

---


