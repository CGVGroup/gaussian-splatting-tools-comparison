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

## 1. Prerequisites

- NVIDIA GPU with **CUDA** support
- PyTorch with CUDA support (tested runtime: CUDA 11.8)
- Python 3.8+ (Conda/Miniconda recommended) (tested: Python 3.10.19)
- Git

---

## 2. Create the Environment

```bash
conda create -n nerfstudio python=3.8 -y
conda activate nerfstudio
```

Install PyTorch compatible with CUDA 11.8:

```bash
pip install torch==2.1.2+cu118 torchvision==0.16.2+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
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

## 3. Dataset Preparation from Video (ffmpeg)

If the dataset is provided as a video rather than a set of images, frames must first be extracted.

```bash
ffmpeg -i {video} -qscale:v 1 -qmin 1 -vf fps={fps} %04d.jpg
```

Where:

- `{video}` → filename or full path to the input video file
- `{fps}` → desired frame extraction rate

The extracted frames are saved as sequentially numbered `.jpg` images (e.g., `0001.jpg`, `0002.jpg`, ...).

### 3.1 FPS Selection Heuristic

The extraction FPS was selected to obtain approximately **150 frames per scene** (151 frames in the benchmark datasets).

---

## 4. Dataset Processing (Automatic COLMAP)

Input directory structure:

```
<dataset>
└── images
    ├── frame_0001.jpg
    ├── frame_0002.jpg
    └── ...
```

Run preprocessing:

```bash
ns-process-data images \
  --data <path_to_images> \
  --output-dir <processed_output>
```

This step automatically performs:

- Feature extraction
- Feature matching
- Bundle adjustment
- Intrinsics refinement
- Dataset formatting

Output structure:

```
<processed_output>
├── images
├── sparse
└── transforms.json
```

---

## 5. Training

The pipeline used in the benchmark is **splatfacto**.

```bash
ns-train splatfacto \
  --data <processed_output> \
  --output-dir <results> \
  --pipeline.model.stop-split-at 20000 \
  --max-num-iterations 30000
```

#### Parameter description

- `splatfacto` → Gaussian Splatting training pipeline implemented in nerfstudio  
- `--data` → path to the processed dataset (output of `ns-process-data` or a COLMAP dataset)
- `--output-dir` → directory where checkpoints, logs and config files are stored
- `--pipeline.model.stop-split-at 20000` → stops Gaussian densification (splitting) after 20k iterations to stabilize training and reduce artifacts
- `--max-num-iterations 30000` → total number of optimization iterations

After training, the folder will contain:

```
<results>
└── splatfacto
    └── <experiment_name>
        ├── config.yml
        ├── nerfstudio_models
        └── viewer
```

The `config.yml` file is required for exporting the reconstruction.

---

## 6. Export

```bash
ns-export gaussian-splat \
  --load-config <config.yml> \
  --output-dir <export_folder>
```

#### Parameter description

- `gaussian-splat` → export format for 3D Gaussian Splatting reconstruction
- `--load-config` → path to the `config.yml` generated after training (contains dataset path, model parameters and checkpoint reference)
- `--output-dir` → directory where the exported reconstruction will be saved

Output:

```
point_cloud.ply
```

---
