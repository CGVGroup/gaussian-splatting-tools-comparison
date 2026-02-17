# nerfstudio — How-To (Training, Export, and Visualization)

This document describes the workflow used for the **nerfstudio (splatfacto)** implementation, including:

- Environment setup (Windows)
- Dataset preparation
- Training
- Export of the Gaussian Splatting reconstruction

---

## References

- **nerfstudio repository**  
  https://github.com/nerfstudio-project/nerfstudio

- **Official documentation**  
  https://docs.nerf.studio/

- **Tutorial videos**
  - https://www.youtube.com/watch?v=3JIpZd5XNAc
  - https://www.youtube.com/watch?v=LhAa1B9CFeY
  - https://www.youtube.com/watch?v=A1Gbycj0bWw

---

## Important Note

During the original experiments, nerfstudio was installed inside an environment where a working `gsplat` installation was already present.

Because of this:

- CUDA extensions were not compiled from scratch
- The installation procedure is not fully reproducible
- Some official installation steps were not required

The installation guide described below follows the official documentation for reproducibility.

---

## 1. Overview

nerfstudio is a neural rendering framework providing tools for data preparation, training and visualization.

The Gaussian Splatting pipeline (`splatfacto`) requires known camera poses and intrinsics.

nerfstudio provides the command `ns-process-data` to process the data into the nerfstudio format.

The workflow therefore consists of two stages:

1. Dataset preparation (`ns-process-data` or existing data with `ns-download-data`)
2. Gaussian Splatting optimization (`ns-train`)

In this work, `ns-process-data` was used.

---

## 2. Prerequisites

- NVIDIA GPU with **CUDA** support
- PyTorch with CUDA support (tested runtime with CUDA 11.8)
- Python 3.8+ (Conda/Miniconda recommended) (tested with Python 3.10.19)
- Git

---

## 3. Create the Environment

Using Conda:

```bash
conda create -n nerfstudio python=3.8 -y
conda activate nerfstudio
python -m pip install --upgrade pip
```

Install PyTorch compatible with CUDA 11.8:

```bash
pip install torch==2.1.2+cu118 torchvision==0.16.2+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
```
Install the cuda-toolkit:

```bash
conda install -c "nvidia/label/cuda-11.8.0" cuda-toolkit
```

Install the torch bindings for tiny-cuda-nn:

```bash
pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
```

Install nerfstudio from pip:

```bash
pip install nerfstudio
```

Or install nerfstudio from source:

```bash
git clone https://github.com/nerfstudio-project/nerfstudio.git
cd nerfstudio
pip install --upgrade pip setuptools
pip install -e .
```

---

## 4. Dataset Processing

In order to run the preprocessing, the command **ns-process-data** is used. The data can be a video or images.

```bash
ns-process-data {images, video} --data {DATA_PATH} --output-dir {PROCESSED_DATA_DIR}
```

Example (images):

```bash
ns-process-data images --data C:\Users\ernes\nerfstudio\data\video-interno\input --output-dir C:\Users\ernes\nerfstudio\data\video-interno-processed
```

---

## 5. Training

The pipeline used in the benchmark is **splatfacto**.

```bash
ns-train splatfacto --data {DATA_PATH} --output-dir {OUTPUT_DIR}
```

Example:

```bash
ns-train splatfacto --data C:\Users\ernes\nerfstudio\data\video-interno-processed --output-dir C:\Users\ernes\nerfstudio\results\video-interno --pipeline.model.stop-split-at 20000 --max-num-iterations 30000
```

#### Parameter description

- `splatfacto` → Gaussian Splatting training pipeline implemented in nerfstudio  
- `--data` → path to the processed dataset
- `--output-dir` → directory where the output is stored
- `--pipeline.model.stop-split-at 20000` → stops Gaussian densification after 20k iterations
- `--max-num-iterations 30000` → total number of optimization iterations

At the end of the terminal is present a link that loads the webviewer.

After training, the folder contains:

```
<results>
└── splatfacto
    └── <experiment_name>
        ├── config.yml
        └── ...
```

---

## 6. Export

The command **ns-export** is used for exporting `.ply` from the `config.yml` file.

```bash
ns-export gaussian-splat --load-config {config.yml} --output-dir {OUTPUT_DIR}
```

Example:

```bash
ns-export gaussian-splat --load-config C:\Users\ernes\nerfstudio\results\video-interno\splatfacto\2026-01-20_131913\config.yml --output-dir C:\Users\ernes\nerfstudio\results\video-interno\exported
```

#### Parameter description

- `gaussian-splat` → export format for 3D Gaussian Splatting reconstruction
- `--load-config` → path to the `config.yml` generated after training (contains dataset path, model parameters and checkpoint reference)
- `--output-dir` → directory where the exported reconstruction will be saved

---
